# How to Obtain RHACM Pre-Release Software

Customers that are part of the RHACM Early Access program can obtain pre-release software versions to test out new features and functionality. Customers report back the experience to their designated Red Hat Field Product Manager who delivers that feedback to Engineering and Product Management teams.  

The following document guides the customer through the process of getting access and deploying those pre-release versions into an OpenShift cluster.

## Accessing RHACM Pre-release versions

1. Go to [Quay.io](https://quay.io) and register for an account.

2. Once a username has been created provide that username to your Red Hat Field Product Manager who will get it associated to the private pre-release registries.
3. In the Quay.io interface in the upper right hand corner click on your username and a drop down with *Account Settings* will appear.  Go into *Account Settings*.
4. On the left hand side of *Account Settings* select the *Gears* icon for user settings.
5. Near the top left of the settings page is a *CLI Password* label followed by a clickable *Generate Encrypted Password* link.  Click this link and type in your quay.io password.
6. This will generate a *Credentials for YourUsername* box that will have various credentials for the user.  Select the *Kubernetes Secret* on the left hand side list.
7. Download the username-secret.yaml file as we will need this for later steps.  
8. Save this file to the host where the `oc` client and OpenShift cluster access will be. 

**Note: Do not proceed to the next steps until the username has been confirmed by Red Hat that it has been given access to the private registry.**

## Depoying a RHACM Pre-release

1. Ensure you have a running OpenShift cluster **without** ACM installed. You'll need access to the `oc` CLI. You may need to make a softlink for `kubectl` to the `oc` client if kubectl does not exist on host.

    ~~~bash
    $ sudo ln -s /usr/local/bin/oc /usr/local/bin/kubectl
    ~~~
    
2. Ensure the machine you're working on has `sed`, `jq`, and `yq` installed and in your `PATH`. 

    You may need to download `yq` from [https://github.com/mikefarah/yq](https://github.com/mikefarah/yq) depending on your distro. Versions may change, but a **non-production** way to do this is:
    
    ~~~bash
    $ wget https://github.com/mikefarah/yq/releases/download/v4.21.1/yq_linux_amd64
    $ chmod +x ./yq_linux_amd64
    $ sudo mv ./yq_linux_amd64 /usr/bin/yq
    ~~~

3. Clone the upstream ACM deploy repository at Github on the machine that has the working oc client and kubeconfig with access to the OpenShift cluster.
  
    ~~~bash
    $ git clone https://github.com/stolostron/deploy.git
    ~~~
    
4. Create a `pull-secret.yaml` file from the contents of `username-secret.yaml` file obtained from Quay.io.  

    The metadata `name:` should be changed to: `multiclusterhub-operator-pull-secret`.  
    
    Place the file inside the deploy/prereqs directory.  The finished file and location should look similar to the below but note the pull-secret itself has been redacted in this example:

    ~~~bash
    $ cat ~/deploy/prereqs/pull-secret.yaml 
    apiVersion: v1
    kind: Secret
    metadata:
      name: multiclusterhub-operator-pull-secret
    data:
      .dockerconfigjson: <PULL-SECRET REDACTED>
    type: kubernetes.io/dockerconfigjson
    ~~~

5. Update the `snapshot.ver` to a known good pre-release snapshot (this value will be provided to you by a Field PM). The updated file will look similar to the one below:

    ~~~bash
    $ cat ~/deploy/snapshot.ver 
    2.5.0-DOWNSTREAM-2022-03-01-06-19-12
    ~~~
6. Apply the following ImageContentSourcePolicy to the cluster:

    ~~~bash
    $ oc apply -f  ~/deploy/addons/downstream/image-content-source-policy.yaml
    ~~~

7. Next pull the current pull secret from the cluster and dump into file:

    ~~~bash
    $ oc get secret/pull-secret -n openshift-config -o json | jq '.data.".dockerconfigjson"' | tr -d '"' | base64 -d > ~/cluster-pull-secret.json
    ~~~
    
8. Extract the pre-release pull secret from the pull-secret.yaml created back in step 10:

    ~~~bash
    $ export PRERELEASE_PULL=$(grep -o '.dockerconfigjson:.*' ~/deploy/prereqs/pull-secret.yaml | cut -f2- -d: | sed 's/^[ \t]*//;s/[ \t]*$//')
    ~~~

9. Convert quay.io to quay.io:443 and dump out to prerelease-secret.json:

    ~~~bash
    $ echo $PRERELEASE_PULL | base64 -d | sed "s/quay\.io/quay\.io:443/g" | tail -n +3 | head -n -2 > ~/prerelease-secret.json
    ~~~
    
10. Merge prerelease-secret.json with existing cluster-pull-secret.json

    ~~~bash
    $ cat ~/cluster-pull-secret.json | jq ".auths += {`cat ~/prerelease-secret.json`}" > ~/merged-pull-secret.json
    ~~~

11. Patch cluster with new merged-pull-secret.json

    ~~~bash
    $ oc patch secret/pull-secret -n openshift-config --type merge --patch '{"data":{".dockerconfigjson":"'$(cat ~/merged-pull-secret.json | tr -d '[:space:]' | base64 -w 0)'"}}'
    ~~~

12. Validate pull-secret from cluster looks correct:

    ~~~bash
    $ oc get secret/pull-secret -n openshift-config -o json | jq '.data.".dockerconfigjson"' | tr -d '"' | base64 -d | python3 -m json.tool
    ~~~

13. Set a few environmental variables for the deploy:

    ~~~bash
    $ export DEBUG=true
    $ export COMPOSITE_BUNDLE=true
    $ export CUSTOM_REGISTRY_REPO="quay.io:443/acm-d"
    $ export DOCKER_CONFIG=`cat ~/deploy/prereqs/pull-secret.yaml |grep dockerconfigjson:|cut -d: -f2|tr -d '[:space:]'`
    $ export QUAY_TOKEN=$(echo $DOCKER_CONFIG | base64 -d | sed "s/quay\.io/quay\.io:443/g" | base64 -w 0)
    ~~~
    
14. Run the deploy process to install prerelease RHACM:

    ~~~bash
    $ cd ~/deploy
    $ ./start.sh --watch
    ~~~
    
    **Note:** Before the deployment starts you'll need to confirm the SNAPSHOT TAG. The script will pause at dialog like the following:
    
    ~~~bash
    Enter SNAPSHOT TAG: (Press ENTER for default: 2.5.0-DOWNSTREAM-2022-03-01-06-19-12)
    ~~~
    
    **Confirm the TAG is correct** and hit ENTER to continue.
    
    **Note:** The deployment process is run in DEBUG mode so you will see a lot of errors and strange conditions displayed. Allow the process to finish. If you hit any issues save the logs (copy and paste from the screen if needed) and share with your Field PM for help!
    
15. Once the `start.sh` deploy script completes the RHACM components are installed and ready to use.
16. You can view the successful installation with the following command:

    ~~~bash
    $ oc get sub -n open-cluster-management
    NAME                        PACKAGE                       SOURCE                CHANNEL
    acm-operator-subscription   advanced-cluster-management   acm-custom-registry   release-2.5
    ~~~

17. You can find the ACM UI (the "multicloud console") by checking for its route:

    ~~~bash
    $ oc get routes -A | grep multicloud
    multicloud-console   multicloud-console.apps.cluster1-example.com          management-ingress   https   reencrypt/Redirect   None
    ~~~

## Uninstall RHACM Pre-release
    
If RHACM needs to be uninstalled just run the `./uninstall.sh` script in `deploy` directory of the git checkout.

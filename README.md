# **ACM High Touch Beta Test Plan**

Many thanks for your participation in the ACM High Touch Beta - we realise that your time is valuable and we very much appreciate the assistance in making our ACM product better. The purpose of this document is to provide guidance for participation in the ACM high touch beta programme, it details the main objectives that we ask our customers to complete, along with links to documentation, examples where required, how to get assistance, and how our HTB customers can provide feedback.

To be clear, the purpose of the HTB is to facilitate access to early-release software with key customers that meet the inclusion criteria that we’ve set. Customers that are included within the programme are doing so on a mutually beneficial basis, more specifically-

1. We’re getting some soak-time for the product within our customer base - customers can test it as a pre-release/beta/early offering with no production support, where it’s somewhat expected that certain features may not be fully developed. It’s one thing for us to test our software internally, with our test workloads, and with our expectations, but it’s another for our customers to test it within their datacentres, with their workloads, with their restrictions, and with their expectations.
2. We’re giving select customers the opportunity to weigh into the product prioritisation - they’ll be testing the software before anyone else; their bugs, RFE’s, suggestions, and feedback will help set expectations and prioritisation of our engineering efforts for the coming releases. In other words, the product (somewhat) gets built to suit their desires.
3. We’ll be agreeing a converged test plan with each customer that’ll be a combination of our “generic test plan”, detailed below, and the customers specific use-case or workload requirements that they’d like to put to the test. This will enable us to receive critical feedback around features, usability, stability, and readiness of our platform, and raise any gaps as RFE’s and/or bugs (including documentation) so that we can make both our current and future releases better.

Below you’ll find a broken out set of major objectives that we’re asking all participants to complete over the next few weeks/months. We’ll be assigning a point of contact (a Field Product Manager) and setting up mechanisms for communication, both for receiving assistance, but also to create a conduit for feedback to flow back into our product teams. We’ll be looking to set up a regular cadence call but also a chat channel for quicker and on-demand communication.

Furthermore, by establishing this relationship, the Field Product Manager can coordinate additional resources from across the organisation to join the sync calls if/when required, e.g. engineering, product management, or executive sponsors.
For each of the tasks below, please report your suggestions, feedback, and any bugs/RFE’s to your assigned Field Product Manager in your weekly cadence calls, or via the chat.



## **Task One: Installation of ACM**

**Task**: In this task you’re going to be asked to deploy Red Hat ACM on an existing OpenShift cluster (must be at least OpenShift 4.3). ACM is deployed as an operator, and therefore you’ll deploy the components using the OpenShift OperatorHub, just like any other operator.

**Documentation**: [Installing instructions for Red Hat Advanced Cluster Management for Kubernetes](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html-single/install/index)

**Installation Video**: https://youtu.be/4keQWJoFl7A

**Success Criteria**: ACM is fully installed, all pods are running, and you can log into the ACM UI (via the exposed service route). Note, when checking the status of the multicluster-hub operator, be sure to select the 'open-cluster-management' project, or you won’t find the operator listed as it’s a “scoped” resource.



## **Task Two: Import an Existing Cluster**

**Task**: In this task you’re going to be asked to import an existing OpenShift cluster using Red Hat ACM. The cluster can be of any type environment: AWS, GCP, Azure, IBM, Vmware or Baremetal (physical). You should be able to import via either the OpenShift Console and/or the OpenShift CLI.

**Documentation**: [Importing a target managed cluster to the hub cluster](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_cluster/importing-a-target-managed-cluster-to-the-hub-cluster)

**Success Criteria**: Once completed with this task the OpenShift cluster should be visible in the ACM UI under the location of Automate Infrastructure -> Clusters. Details about the cluster should also be visible.



## **Task Three: Deploy a New Cluster**

**Task**: In this task you will be creating a new OpenShift cluster with ACM. This will require you to create first a provider connection and then complete the create a cluster portion using the provider connection configured. Deployment of any supported cluster type can be used.

**Documentation (Provider Connection)**: [Creating a provider connection](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_cluster/creating-a-provider-connection) 

**Documentation (Create Cluster)**: [Creating a cluster](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_cluster/creating-a-cluster) 

**Success Criteria**: Upon successful completion of task ACM should have deployed a OpenShift cluster on the provider type chosen. Running a few oc commands on the newly deployed cluster can validate it is up and operational: oc get nodes, oc get co



## **Task Four: Launch OCP Console from ACM**

**Task**: In this task after you have completed either installing or importing a OpenShift cluster via ACM you should be able to launch the OpenShift clusters Console UI via Automate Infrastructures -> Clusters.

**Documentation**: [Observability in the console](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/web_console/web-console#observability-in-the-console)

**Success Criteria**: Ability to launch the OpenShift Console from ACM and successfully login and view details of the cluster.



## **Task Five: Upgrade an OCP Cluster**

**Task**: In this task you can use ACM Console to upgrade those clusters to the latest minor version that is available in the version channel that the managed cluster uses.

**Documentation**: [Upgrading your cluster](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_cluster/upgrading-your-cluster)

**Success Criteria**: From the ACM Console under Automated Infrastructure -> Cluster one can select to upgrade cluster from one minor version to another. During the upgrade process one may see the upgrade cycling from failed/unknown/upgrading as it takes approximately 30-45 minutes depending on size and resources of the cluster. During the upgrade process one can also use the Launch to console action to view the upgrade experience. Cluster is upgraded from one minor to the selected target minor 

> **NOTE**: Upgrade the major version of a cluster using a [configuration policy.](https://github.com/open-cluster-management/policy-collection/blob/master/community/CM-Configuration-Management/policy-upgrade-openshift-cluster.yaml); more details on using configuration policies is in a later task.



## **Task Six: Deploy Application Resources & Create a Placement Rule**

**Task**: In this task we will deploy an application with a subscription, channel and placement rule. We will use the Create Application action in the ACM Console UI to create the application using the dev app yaml provided below.

**Documentation**: [Manage applications](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_applications/index)

**Sample Application Yaml**: [Sample application code](https://github.com/open-cluster-management/deploy/blob/master/demo/app/devapp/devapp.yaml) 

**Success Criteria**: Success of this task is based on the ability to create an application action and verify the app shows in the table. Further we should be able to validate that the channel was created and shows up in the pipeline.



## **Task Seven: Modify a Placement Rule**

**Task**: Ensure you have at least one cluster with a specific label (such as “dev”, “prod”, “test”, etc). Modify an Application’s “PlacementRule” section to use a specific cluster by updating or creating the **matchLabels** directive. Review the application Topology and see that there is now a placement to the new clusterObserve the app placement from the OCP cluster itself. View it side-by-side with your ACM console and have the pods view open with the project selected.

**Documentation**: [Placement rules](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_applications/managing-applications#placement-rules)

**Success Criteria**: The application is moved to the cluster with the label specified.



## **Task Eight: Update Application Version**

**Task**: Update an application from a current version to a newer version. Update the packageFilter setting for your application (ie change the version label to a newer release).

**Documentation**: [Update application](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.0/html/manage_applications/managing-applications#application)

**Success Criteria**: The application is updated to the newer version.



## **Task Nine: Deploy an application to multiple clusters at once**

**Task**: Deploy an application to multiple clusters to test availability across regions/infrastructure. Using the provided repo as an *example* experiement with placing your own apps on different public clouds and/or clusters.

**Documentation**: [Demonstrate placement algorithm](https://github.com/open-cluster-management/demo-subscription-gitops/blob/master/placement/README.md)

**Success Criteria**: The application is successfully deployed to multiple clusters and/or providers.



## **Task Ten: Security Policy Creation and Automated Remediation**

**Task**: Create a security policy that checks for a specific resource and inform on its presence. Create a remediation policy to take action when a violation is detected and correct/mitigate it.

**Documentation**: 

* [Security and RHACM (overview)](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/security/security)

* [Manage Security policies](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/security/security#manage-security-policies)

**Overview**: https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_applications/managing-applications

**Policies**: 

* [A collection of policy examples for Open Cluster Management](https://github.com/open-cluster-management/policy-collection)

* [Community contributed policies to try](https://github.com/open-cluster-management/policy-collection/tree/master/community)

**Success Criteria**: A policy alerts when criteria are matched but no action is taken. Additionally the violation is clearly and comprehensively reported and reviewable via the UX and CLI and/or reporting mechanisms.A policy is triggered and the result is automated remediation without any additional action. Additionally the violation and remediation are clearly and comprehensively reported and reviewable via the UX and CLI and/or reporting mechanisms.



## **Task Eleven: General Observability**

**Task**: In this task after deploying and/or importing a OpenShift cluster, interact with the key observability areas of the ACM Console UI: Overview, Search, Virtual Web Terminal and Topology views

**Documentation**: [Web console overview and usage](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/web_console/web-console)

**Success Criteria**: You are able to view and easily find the Clusters, Applications, and Topology views and observe that they accurately and clearly reflect the state of your cluster(s). You are able to use the search feature to find resources across all managed clusters including the use of filters to “drill down” to very specific information sets.You are able to save complex search queries for reuseYou have been able to explore the Visual Web Terminal to run basic OC commands on both your hub and managed clusters in the same way as from a standard CLI


## **Task Twelve: Global Query view with Grafana**

**Task**: Review the new out of the box multi cluster health monitoring dashboards. Create custom queries using PromQL (Prometheus Query Language).

**Documentation**: [PromQL Cheat sheet (not Red Hat supported)](https://promlabs.com/promql-cheat-sheet/)

**Success Criteria**: Useful queries can be easily built. We are curious to see what queries you build and how you use them. And also other ways you’d like to be able to access this data. Please share your findings with us.

## **Next Steps: Comprehensive Scenario-Driven Labs**

**Task**: We have comprehensive publicly available labs that help you to really understand RHACM. Once you’ve completed the basic task lists for the High Touch Beta it’s worth delving into some of these labs to try some new things on your clusters (or even try some deployments to infra you may not use normally!). We are eager to see how customers use RHACM and these labs represent a solid cross section of tasks we see as foundational. But we need your feedback! Do they work? Can they be improved? Minimally, try applying the concepts in each lab on your own deployments and record your feedback for review by our Product Management and Engineering teams! This is your chance to share directly with the teams that shape *and create* the product! Have your say!

**MultiCloud Cluster Deployments with ACM**: In this series of labs you’ll install an ACM Hub on AWS and then provision managed clusters on AWS, Azure, and GCP. You’ll get to deploy a sample app and explore load balancing across the clusters. Bonus: why extend the concepts of the lab by adding vmware or physical OCP clusters in your datacentre! We’d like to see how these new deployment features work in our customer’s environments.Introduction to GitOps and Policies with ACM: Continue the policy task by diving deep into the world of governance and control! You’ll deploy policies to multiple clusters and cover important topics like DR and infra as code. Application Portability: Use GitOps to deploy a complex application across multiple clusters. This lab provides a “textbook” deployment scenario. Run through it and see how it would apply to your own applications? Are there gaps? What can be improved in the product to make it work better for you!?

**Documentation**: https://github.com/open-cluster-management/labs

**Success Criteria**: Complete the labs to learn the basic steps and concepts for core RHACM tasks. But we don’t just want you to finish them, we want to know where there are gaps for your company workflows. How can these processes be improved? Take lots of notes and let’s sit down with Product Management and Engineering to share your experiences!Bonus Success: contribute to the labs with improvements and more content!



# End
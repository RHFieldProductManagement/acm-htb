# **Red Hat Advanced Cluster Management Early Access FAQs**

The following a collection of common questions and answers related to the Red Hat Advanced Cluster Management (RHACM) Early Access Program.

## **Overview Questions**

* Question: What is the RHACM release cycle in relation to the OpenShift release cycle?
  - Answer: RHACM is attempting to stay in sync with OCP quarterly releases.  The RHACM releases are usually about 3 weeks after the OCP release to accommodate testing of the latest OCP GA.   Example release cadence: RHACM 2.1 = OCP 4.6, RHACM 2.2 = OCP 4.7, RHACM 2.3 = OCP 4.8 
* Question: Do we suggest a completely dedicated OCP install for RHACM?
  - Answer: It is a best practice but not a hard requirement.
* Question: Where can I find the RHACM supportability matrix?
  - Answer: [Red Hat Advanced Cluster Management for Kubernetes 2.2 Support Matrix](https://access.redhat.com/articles/5799561)

## **Observability Questions**

## **Cluster Lifecycle Questions**

## **Application Lifecycle Questions**

* Question: How can I use Submariner to connect two managed RHACM clusters together?
  - Answer: [Connecting managed clusters with Submariner in Red Hat Advanced Cluster Management for Kubernetes](https://www.openshift.com/blog/connecting-managed-clusters-with-submariner-in-red-hat-advanced-cluster-management-for-kubernetes)
* Question: When creating subscriptions to be delivered to managed clusters what namespace should they reside in on the hub cluster?
  - Answer: The answer really depends on how the customers want to organize things. By default, when a hub subscription deploys subscribed applications to      target clusters, the applications are deployed to that subscription namespace. Unless of course those app yamls specifically define its namespace.
This behaviour can be configure but if you go by the default behaviour, it would imply the subscription lives in the same namespace as the namespace scope resources it deploys and manage.In this setup, the hub will have a namespace that matches the spoke cluster namespace both with their own subscription (hub vs standalone). This is easy to mentally visualize.But if there are a lot of apps that will live in their own namespaces then the hub will also have tons of namespaces so the answer really depends on how the customer want to set it up.

## **Governance, Risk & Compliance Questions**

* Question: Where can I find more details on integrating Gatekeeper with RHACM?
  - Answer: [Integrating Gatekeeper with Red Hat Advanced Cluster Management for Kubernetes](https://www.openshift.com/blog/integrating-gatekeeper-with-red-hat-advanced-cluster-management-for-kubernetes)
* Question: Where can I find more details on the policy framework?
  - Answer: [Governance Policy Framework](https://github.com/open-cluster-management/governance-policy-framework)
  - Answer: [Policy Framework Video](https://youtu.be/13TOnhu4ex8)
* Question: How can I manage configuration via policy based governance?
  - Answer: [Implement Policy-based Governance Using Configuration Management of Red Hat Advanced Cluster Management for Kubernetes](https://www.openshift.com/blog/implement-policy-based-governance-using-configuration-management-of-red-hat-advanced-cluster-management-for-kubernetes)
* Question: Does deleting a policy remove the changes a policy applied to the managed cluster?
  - Answer: [Revert Policy Applied Changes to Managed Cluster](https://access.redhat.com/solutions/6005481)

## **Miscellaneous Questions**

* Question: When can I expect to see RHACM curriculum in the Red Hat Learning Portal?
  - Answer: RHACM are tenatively scheduled to be in the Red Hat Learning Portal the second half of 2021. 

## **RFE's & Bugzillas**

* [RFE Enable Ansible as deployment method](https://issues.redhat.com/browse/ACM-713)
* [RFE Enable Ansible as cluster upgrade lifecycle method](https://issues.redhat.com/browse/ACM-735)
* [RFE AppSub Resilience from channel [git, helm, objectstore]](https://issues.redhat.com/browse/ACM-721)
* [RFE Provide a "templatized" way to provide the value of the secret or config map](https://issues.redhat.com/browse/ACM-240)
* [BZ Policy Gitops appears to be missing from documentation](https://bugzilla.redhat.com/show_bug.cgi?id=1942124)
* [BZ Hive based OCP IPI baremetal installation fails to connect to API VIP port 22623](https://bugzilla.redhat.com/show_bug.cgi?id=1936443)

# **Red Hat Advanced Cluster Management Early Access FAQs**

The following a collection of common questions and answers related to the Red Hat Advanced Cluster Management (RHACM) Early Access Program.

## **Overview Questions**

* Question: What is the RHACM release cycle in relation to the OpenShift release cycle?
  - Answer: RHACM is attempting to stay in sync with OCP quarterly releases.  The RHACM releases are usually about 3 weeks after the OCP release to accommodate testing of the latest OCP GA.   Example release cadence: RHACM 2.1 = OCP 4.6, RHACM 2.2 = OCP 4.7, RHACM 2.3 = OCP 4.8 
* Question: Do we suggest a completely dedicated OCP install for RHACM?
  - Answer: It is a best practice but not a hard requirement.

## **Observability Questions**

## **Cluster Lifecycle Questions**

## **Application Lifecycle Questions**

* Question: How can I use Submariner to connect two managed RHACM clusters together?
  - Answer: [Connecting managed clusters with Submariner in Red Hat Advanced Cluster Management for Kubernetes](https://www.openshift.com/blog/connecting-managed-clusters-with-submariner-in-red-hat-advanced-cluster-management-for-kubernetes)

## **Governance, Risk & Compliance Questions**

* Question: Where can I find more details on integrating Gatekeeper with RHACM?
  - Answer: [Integrating Gatekeeper with Red Hat Advanced Cluster Management for Kubernetes](https://www.openshift.com/blog/integrating-gatekeeper-with-red-hat-advanced-cluster-management-for-kubernetes)
* Question: Where can I find more details on the policy framework?
  - Answer: [Governance Policy Framework](https://github.com/open-cluster-management/governance-policy-framework)
  - Answer: [Policy Framework Video](https://youtu.be/13TOnhu4ex8)
* Question: How can I manage configuration via policy based governance?
  - Answer: [Implement Policy-based Governance Using Configuration Management of Red Hat Advanced Cluster Management for Kubernetes](https://www.openshift.com/blog/implement-policy-based-governance-using-configuration-management-of-red-hat-advanced-cluster-management-for-kubernetes)

## **Miscellaneous Questions**

* Question: When can I expect to see RHACM curriculum in the Red Hat Learning Portal?
  - Answer: RHACM are tenatively scheduled to be in the Red Hat Learning Portal the second half of 2021. 

## **RFE's & Bugzillas**

* [RFE Enable Ansible as deployment method](https://issues.redhat.com/browse/ACM-713)
* [BZ Policy Gitops appears to be missing from documentation](https://bugzilla.redhat.com/show_bug.cgi?id=1942124)

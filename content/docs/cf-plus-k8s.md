+++
title = "CF plus Kubernetes"
weight = "3"
+++

Often the answer for an organziation is not "Cloud Foundry or Kubernetes" but rather "Cloud Foundry and Kubernetes". It is a powerful combination. Cloud Foundry can provide significant development and operational efficiency to software development organizations. Kubernetes can provide rest. They can be deployed side by side, or Cloud Foundry can run on Kubernetes.




## Deploying CF on Kubernetes

Because Kubernetes can run any workload distributed as container images, Kubernetes can run Cloud Foundry itself. When installed on Kubernetes, **Cloud Foundry presents a streamlined self-service interface for application developers to deploy apps onto Kubernetes**, whilst allowing the Kubernetes **operators to maintain more control** over what is running on the system. For any workload not suited for Cloud Foundry, Kubernetes can be used directly.

### Benefits

By running Cloud Foundry on Kubernetes, you can leverage the knowledge and skills of operators who already know Kubernetes and the Cloud Native Computing Foundation ecosystem of tools.

Compared to running Cloud Foundry on virtual machines (via [BOSH](https://bosh.io)), Cloud Foundry on Kubernetes can make more efficient use of resources, and potentially perform platform-level upgrades faster. While this capability is maturing quickly, it is currently still undergoing significant development to bring feature parity when compared to running CF on virtual machines.

### Projects

Currently, two open source projects aim to bring the `cf push` experience to Kubernetes:

**[KubeCF](https://github.com/cloudfoundry-incubator/kubecf)** is currently the most widely-adopted Cloud Foundry distribution available for Kubernetes that you can install yourself. SUSE provide a commercial version called [SUSE Cloud Application Platform](https://www.suse.com/products/cloud-application-platform/), and IBM offer the KubeCF-based [IBM CFEE](https://www.ibm.com/uk-en/cloud/cloud-foundry) too.

**[cf-for-k8s](https://github.com/cloudfoundry/cf-for-k8s)** is an on-going project that is repackaging Cloud Foundry for Kubernetes, and also switching some internal components for more Kubernetes-native equivalents. At the time of writing it is not ready for production use, but is making great progress.

It is the intention of the Cloud Foundry community and members of the Kubernetes SIG that KubeCF and cf-for-k8s converge. Plus, as the Cloud Foundry CLI and API are standardised and certified, you could easily switch between different [certified distributions](https://www.cloudfoundry.org/thefoundry/#cert-distros) if you wanted to.

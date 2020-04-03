+++
title = "How do Cloud Foundry and Kubernetes work together?"
weight = "20"
+++

Cloud Foundry can be deployed on top of Kubernetes - you don't have to choose between the two.

Because Kubernetes can run any workload distributed as container images, Kubernetes can run the components of Cloud Foundry itself.

When installed on Kubernetes, Cloud Foundry presents a streamlined self-service interface for application developers to deploy apps onto Kubernetes, whilst allowing the Kubernetes operators to maintain more control over what is running on the system.

## Benefits

By running Cloud Foundry on Kubernetes, you can leverage the knowledge and skills of operators who already know Kubernetes and the Cloud Native Computing Foundation ecosystem of tools.

Compared to running Cloud Foundry on VMs (via BOSH), Cloud Foundry on Kubernetes makes more efficient use of resources, and performs platform-level upgrades faster.

## Distributions

**[KubeCF](https://github.com/cloudfoundry-incubator/kubecf)** is currently the most widely-adopted Cloud Foundry distribution available for Kubernetes that you can install yourself. SUSE provide a commercial version called [SUSE Cloud Application Platform](https://www.suse.com/products/cloud-application-platform/), and IBM offer the KubeCF-based [IBM CFEE](https://www.ibm.com/uk-en/cloud/cloud-foundry) too.

**[cf-for-k8s](https://github.com/cloudfoundry/cf-for-k8s)** is an on-going project that is repackaging Cloud Foundry for Kubernetes, and also switching some internal components for more Kubernetes-native equivalents. At the time of writing it is not ready for production use, but is making great progress.

It is the intention of the Cloud Foundry community and members of the Kubernetes SIG that KubeCF and cf-for-k8s converge. Plus, as the Cloud Foundry CLI and API are standardised and certified, you could easily switch between different [certified distributions](https://www.cloudfoundry.org/thefoundry/#cert-distros) if you wanted to.

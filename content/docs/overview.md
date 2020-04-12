+++
title = "Overview"
weight = "1"
+++

While we've covered "What is Cloud Foundry" in another tutorial, and there are plenty of great resources explaining Kubernetes, let's summarize them in the context of each other. Both are open-source projects.

* **Cloud Foundry** (CF) is a Platform-as-a-Service that makes it easy to deploy and operate _applications_. The [Cloud Foundry Foundation](https://cloudfoundry.org) oversees the project. 

* **Kubernetes** (k8s) is an infrastructure platform for automating deployment, scaling, and management of _containerized workloads_. The [Cloud Native Computing Foundation](https://www.cncf.io/) oversees the project. 

In short, Cloud Foundry is an opinionated application-focused platform, while [Kubernetes is a platform for building platforms](https://twitter.com/kelseyhightower/status/935252923721793536?s=20). 

## Cloud Foundry and Applications

Cloud Foundry makes it easy to run custom applications in the cloud (and keep them running). Built-in, out of the box automation makes this nearly effortless. Cloud Foundry is often used to run code built by the same organization that is deploying it, rather than off-the-shelf software.

However, Cloud Foundry is not a general-purpose compute platform. You wouldn't run databases _on_ Cloud Foundry; instead, you'd connect your apps on Cloud Foundry to databases running elsewhere.

## Kubernetes and Containers

Kubernetes is designed to be as generic as possible. It aims to run any workload defined in an [OCI (Open Container Initiative)](https://www.opencontainers.org/) container image. Kubernetes can run the same applications as Cloud Foundry (albeit with extra configuration) but is also suitable for running stateful workloads like containerized databases. 

## Cloud Foundry on Kubernetes

Because Kubernetes is designed to be as generic as possible, you can run the components of Cloud Foundry itself _on_ Kubernetes!

## Cloud Foundry on Virtual Machines

The Cloud Foundry project began years before Kubernetes, and so has historically been deployed on virtual machines (VMs) using a tool called [BOSH](https://bosh.io). You will find occasional references to Cloud Foundry on VMs and to BOSH throughout this guide.
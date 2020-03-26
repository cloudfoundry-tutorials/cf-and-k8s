+++
title = "What are Cloud Foundry and Kubernetes?"
weight = "5"
+++

Whilst we've covered "What is Cloud Foundry" in another tutorial, and there are plenty of great resources explaining Kubernetes, let's summarise them in context of each other.

* **Cloud Foundry** is a Platform-as-a-Service that makes it easy to deploy and operate applications.
* **Kubernetes** is an open-source system for automating deployment, scaling, and management of containerized applications. It features an extensible API that makes it a great way to declaratively manage a wide variety of things.

## Overview

The following [diagram from EngineerBetter](https://github.com/EngineerBetter/k8s-is-not-a-paas) summarises the _out of the box_ functionality provided by a traditional IaaS, Cloud Foundry deployed to virtual machines with BOSH, Kubernetes, hosted Kubernetes services like Google's GKE, and Cloud Foundry deployed on top of Kubernetes:

![alt text](https://github.com/EngineerBetter/k8s-is-not-a-paas/raw/master/iaas-kubes-paas.svg?raw=true&sanitize=true "Comparison of Cloud Foundry and Kubernetes functionality")

The diagram shows that:

* Cloud Foundry fits very well on top of Kubernetes
* Kubernetes can be extended to offer the same functionality as Cloud Foundry
* Cloud Foundry can be deployed to virtual machines with BOSH if an organisation does not use or is not yet using Kubernetes

## Similarities

Both systems:

* run applications in **containers**
* run applications that are packaged as **Docker images**
* run very **large production workloads** some of the world's biggest organisations
* can be run **on-premises** or in the **public cloud**
* mean your app developers do not need to know which IaaS (AWS, Google Cloud, VMware, etcetera) is being used
* are **open source** and owned by **independent foundations**

## Differences

### Intended Goals

**Cloud Foundry** **improves developer productivity** by making applications easy to deploy and operate.

**Kubernetes** allows users to **build developer platforms** by providing an extensible set of interoperable components.

By way of analogy, one can imagine that **Cloud Foundry is a doll's house**, and **Kubernetes is box of building blocks** from which you can build a doll's house.

### Design Approach

**Cloud Foundry** is an **opinionated set of components** that are designed and distributed to work together.

**Kubernetes** is a **flexible and extensible** system with a huge range of open source components from which you can choose, install, configure and maintain.

### Built-in Functionality

**Cloud Foundry** embodies an **opinionated workflow**, and automates the building of container images from application code, configuration of HTTPS access to your apps, and comes pre-configured with multi-tenancy access controls that are suitable for use in banks and governments.

**Kubernetes** offers a huge amount of flexibility, and **can be configured and extended to support virtually any workflow**. As such, you could assemble your own set of components on Kubernetes that replicate the functionality of Cloud Foundry.

### Personas

**Cloud Foundry** has **different user experiences** for **operators** and for **application developers**. It presents a simplified interface to developers, whilst offering controls to operators so that they can restrict what is running on their platform.

*Kubernetes* presents the same user interface for all its users. Out-of-the-box, **application developers and operators interact with Kubernetes the same way**.

### Operator Control

**Cloud Foundry** has built-in functionality that **allows operators to maintain control** of the base images that are used to build application containers, so that they can know exactly what code is running in their platform. Additionally, **operators can update these base images without involving application developers**.

Many **technologies are available for Kubernetes to restrict container images** that are run on the platform, and to scan for vulnerabilities at runtime. Rebuilding container images often requires the execution of a continuous integration pipeline, and/or the involvement of application developers.

### Flexibility

**Cloud Foundry** supports an opinionated workflow and purposefully **hides many implementation details** from application developers.

**Kubernetes** allows full access to all implementation details, allowing **custom workflows and deployment strategies** to be implemented.

For example, it is much easier in Kubernetes to schedule workloads onto machines with particular attributes, such as those with GPUs installed. Whilst similar outcomes can be achieved with Cloud Foundry,

### Role-Based Access Control (RBAC)

Both Cloud Foundry and Kubernetes offer RBAC.

**Cloud Foundry** comes **pre-configured** with a set of roles and permissions that allow untrusted tenants to share the same platform.

**Kubernetes** has support for RBAC, and you can **create your own set of roles and permissions** to suit your individual requirements.

### Secure by Default

**Cloud Foundry has all available security options enabled by default** (ie seccomp, apparmor, dropping root capabilites, et al). You cannot, for example, run privileged workloads in Cloud Foundry.

**Kubernetes** is more flexible by default and **allows privileged workloads** to be run on the cluster, and further restrictions can be layered in.

## Conclusion

* Kubernetes can do everything that Cloud Foundry does - you get to choose, install, configure and maintain components yourself.
* Cloud Foundry can _not_ do everything that Kubernetes can do - it is an opinionated set of components that are designed and distributed together, and work out of the box.

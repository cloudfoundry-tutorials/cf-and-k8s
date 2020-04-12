+++
title = "Comparing CF and Kubernetes"
weight = "2"
+++

Cloud Foundry is supplemental to Kubernetes - you can install Cloud Foundry in _addition_ to, or _on_, Kubernetes to make app developers more productive.

Application developers could use Kubernetes _without_ Cloud Foundry. The opposite is also true: Cloud Foundry has existed for longer than Kubernetes, so it's also possible to use Cloud Foundry _without_ Kubernetes.

In this section, we explore how the two systems differ from and resemble each other.

## Overview

The following [diagram from EngineerBetter](https://github.com/EngineerBetter/k8s-is-not-a-paas) summarises the _out of the box_ functionality provided by a traditional IaaS, Cloud Foundry deployed to virtual machines with BOSH, Kubernetes, hosted Kubernetes services like Google's GKE, and Cloud Foundry deployed on top of Kubernetes:

![alt text](https://github.com/EngineerBetter/k8s-is-not-a-paas/raw/master/iaas-kubes-paas.svg?raw=true&sanitize=true "Comparison of Cloud Foundry and Kubernetes functionality")

The diagram shows that:

* Cloud Foundry can compliment Kubernetes by providing a single distribution of additional functionality that is pre-configured to work together
* Kubernetes can be extended to offer the same functionality as Cloud Foundry
* Cloud Foundry can still be deployed to virtual machines with BOSH. This is still the most common deployment methodology today, though the focus is on integrating Cloud Foundry and Kubernetes more robustly.

## Similarities

There are many similarities between Cloud Foundry and Kubernetes. Both systems:

* run applications in **containers**
* run applications that are packaged as **Docker images**
* run very **large production workloads** some of the world's biggest organizations
* can be run **on-premises** or in the **public cloud**
* mean your app developers do not need to know which IaaS (AWS, Google Cloud, VMware, etcetera) is being used
* are **open source** and governed by **independent foundations**

## Differences

Cloud Foundry and Kubernetes aim to serve two different levels of abstraction. Therefore, we highlight some of the differences below.

### Intended Goals

- **Cloud Foundry** **improves developer productivity** by making applications easy to deploy and operate.

- **Kubernetes** allows users to **build developer platforms** by providing an extensible set of interoperable components.

By way of analogy, one can imagine that **Cloud Foundry is a doll's house**, and **Kubernetes is a box of building blocks** from which you can create a doll's house.

### Design Approach

- **Cloud Foundry** is an **opinionated set of components** that are designed and distributed to work together.

- **Kubernetes** is a **flexible and extensible** system with a wide range of open source components from which you can choose, install, configure and maintain.

### Built-in Functionality

- **Cloud Foundry** embodies an **opinionated workflow**, and automates the building of container images from application code, configuration of HTTPS access to your apps, and comes pre-configured with multi-tenancy access controls that are suitable for use in banks and governments.

- **Kubernetes** offers a considerable amount of flexibility, and **can be configured and extended to support virtually any workflow**. As such, you could assemble your own set of components on Kubernetes that replicate the functionality of Cloud Foundry.

### Exposed Complexity

- **Cloud Foundry hides implementation details**. This is why Cloud Foundry talks in terms of apps, rather than containers. It abstracts away the complexity inside the platform, presenting a simpler, more straightforward interface with a significantly lower barrier of entry for users.

- **Kubernetes hides nothing**, and exposes its full power, flexibility and complexity to all users. Many Kubernetes enthusiasts enjoy revelling in the implementation details, and curating sets of components that work together to create a productive platform.

### Personas

- **Cloud Foundry** has **different user experiences** for **operators** and for **application developers**. It presents a simplified interface to developers while offering controls to operators so that they can restrict what is running on their platform.

- **Kubernetes** presents the same user interface for all its users. Out-of-the-box, **application developers and operators interact with Kubernetes the same way**.

### Operator Control

- **Cloud Foundry** has built-in functionality that **allows operators to maintain control** of the base images that are used to build application containers, so that they can know exactly what code is running in their platform. Additionally, **operators can update these base images without involving application developers**. Similarly, operators can control all aspects of self-service, on-demand provisioning developers are allowed to execute. The list includes marketplace services, runtime components, customized routes, deployment domains, etc.

- Many **technologies are available for Kubernetes to restrict container images** that are run on the platform, and to scan for vulnerabilities at runtime. Rebuilding container images often requires the execution of a continuous integration pipeline, and/or the involvement of application developers. 

### Flexibility

- **Cloud Foundry** supports an opinionated workflow and purposefully **hides many implementation details** from application developers. This approach yields significant speed and velocity to those adopting the opinions of the platform.

- **Kubernetes** allows full access to all implementation details, allowing **custom workflows and deployment strategies** to be implemented. This approach yields significant flexibility at the cost of operational overhead and custom platform development (and maintenance.)

### Role-Based Access Control (RBAC)

Both Cloud Foundry and Kubernetes offer RBAC.

- **Cloud Foundry** comes **pre-configured** with a set of roles and permissions that allow untrusted tenants to share the same platform. These built-in roles are part of Cloud Foundry's opinion.

- **Kubernetes** has support for RBAC, and you can **create your own set of roles and permissions** to suit your specific requirements.

### Secure by Default

- **Cloud Foundry has all available security options enabled by default** (i.e., seccomp, apparmor, dropping root capabilites, et al). You cannot, for example, run privileged workloads in Cloud Foundry.

- **Kubernetes** is more flexible by default and **allows privileged workloads** to be run on the cluster. Additionally, further restrictions can be layered implemented.

## Conclusion

In conclusion, Cloud Foundry's significant agility and ease of use come from high levels of built-in automation and the implementation of cloud-native opinions. However, Cloud Foundry is not a general compute platform and therefore, can't run all workloads.

On the flip side, Kubernetes is far more flexible. However, you have to implement, automate, and operate the platform solutions and constructs on top of Kubernetes yourself. Essentially, you have to build a platform that will look an awful lot like Cloud Foundry.

Therefore, it is often best to use Cloud Foundry _and_ Kubernetes together, thereby reaping the efficiency, compliance, and security gains built into Cloud Foundry for the majority of your application workloads while maintaining the flexibility and extensibility of the Kubernetes ecosystem for the rest. You can even gain efficiency in the Cloud Foundry platform itself by running it on top of Kubernetes. We explore these options in the next sections of the tutorial.

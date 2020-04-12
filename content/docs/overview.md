+++
title = "Overview"
weight = "1"
+++

While we've covered "What is Cloud Foundry" in another tutorial, and there are plenty of great resources explaining Kubernetes, let's summarize them in the context of each other. Both are open-source projects.

* **Cloud Foundry** (CF) is a Platform-as-a-Service that makes it easy to deploy and operate _applications_. The [Cloud Foundry Foundation](https://cloudfoundry.org) oversees the project. 

* **Kubernetes** (k8s) is an infrastructure platform for automating deployment, scaling, and management of _containerized workloads_. The [Cloud Native Computing Foundation](https://www.cncf.io/) oversees the project. 

In short, Cloud Foundry is an opinionated application-focused platform, while [Kubernetes is a platform for building platforms](https://twitter.com/kelseyhightower/status/935252923721793536?s=20). 


## Focus

Cloud Foundry and Kubernetes differ in their focus. The focus of Cloud Foundry is applications, while the focus of Kubernetes is containers. When asked to compare Cloud Foundry and Kubernetes, Chip Childers, Executive Director of the Cloud Foundry Foundation had this to say:

	"First, equating the Cloud Foundry experience to a Kubernetes experience is like equating apples and walnuts. They're not at all related. Kubernetes is all about being an infrastructure abstraction, but that's not optimized for developers, and it has a more broadly applicable set of use cases -- you can take a legacy app, slap it into a container and run it on Kubernetes; you could craft your own containers that are for a more modern architecture and run that on Kubernetes; and a whole bunch of things in between. The Cloud Foundry experience is focused on optimizing for the developer that's writing custom software for business or, in many cases, government applications. It's all about custom code."

### Cloud Foundry and Applications

Cloud Foundry is focused on brining a world class, cloud-native developer experience to its users. Deploying applications (and keeping them running) on Cloud Foundry is simple as the out of the box functionality makes this nearly effortless. 

However, Cloud Foundry is not a general-purpose compute platform. It is intended to run custom developed applications, not run _any_ workload in any cloud. You wouldn't run databases _on_ Cloud Foundry; instead, you'd connect your apps on Cloud Foundry to databases running elsewhere. Similarly, most commercial off the shelf products (COTS), like Microsoft Exchange, are not ideal candidates for Cloud Foundry though more and more COTS vendors have ported their platforms to run on Cloud Foundry.

### Kubernetes and Containers

Kubernetes is a modern infrastructure abstraction designed to be as generic as possible. It aims to run any workload defined in an [OCI (Open Container Initiative)](https://www.opencontainers.org/) container image. Kubernetes can run the same applications as Cloud Foundry (albeit with significant extra work and configuration) but is also suitable for running stateful workloads like containerized databases or containerized COTS solutions.

## So Cloud Foundry or Kubernetes?

Because Cloud Foundry focuses on the developer experience, it is often the best platform for custom software organizations. But Cloud Foundry stands to gain from the efficiencies and capabilities of Kubernetes. And because Cloud Foundry is not suitable for every workload, organizations need _something else_. For most organizations, this is, or will soon be, Kubernetes. Therefore, it isn't a question of "Cloud Foundry or Kubernetes" but rather "Cloud Foundry _and_ Kubernetes." 

The Cloud Foundry community is aligned in the mission to bring the world-class developer experience to Kubernetes. Traditionally, Cloud Foundry has been deployed on virtual machines (VMs) using a tool called [BOSH](https://bosh.io). But now, you can deploy Cloud Foundry on Kubernetes, gaining efficiencies in the underlying infrastructure and capitalizing on the Kubernetes skills already in your organization. 

In this model, the majority of developers can work at the Cloud Foundry layer, gaining efficiency, security, and speed. For the use cases currently outside of Cloud Foundry's purview, smaller groups of developers can fall back to the lower level primitives in Kubernetes as workloads dictate. Over time, more of the Kubernetes primitives will continue to be integrated and encapsulated in the developer-centric approach of Cloud Foundry, allowing ever more workloads to run via Cloud Foundry on Kubernetes.

We will highlight the projects bringing the `cf push` (the deploy command) experience to Kubernetes later in this tutorial.
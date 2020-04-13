+++
title = "When should I use Cloud Foundry?"
weight = "3"
+++

Not every technology is suitable in every use case. In this section we briefly list some of the ways that you can tell if Cloud Foundry on Kubernetes is right for you.

## Cloud Foundry on Kubernetes

You should consider using Cloud Foundry on Kubernetes when you want:

* developers to have a **simplified experience**, and not need to learn all the details of Kubernetes
* **secure defaults** that prevent privileged workloads from running
* secure **multi-tenancy** by default, with pre-configured role-based access control
* operators to be able to control what is running in their cluster, and **patch images without developer involvement**
* to save the costs of building your own platform

## Kubernetes without Cloud Foundry

You should consider using Kubernetes _without_ Cloud Foundry when you want:

* to invest in building your own platform
* to **choose, install, configure and maintain** all the components of your platform
* developers to have access to the **full power and complexity** of Kubernetes
* to **design your own security model**
* to run more **off-the-shelf** software than in-house software
* to exploit a mature continuous integration/deployment practice that can automate the rebuilding of patched images
* **low-level control** over workloads, for example accessing GPUs for machine-learning

These options need not be mutually exclusive. Perhaps you might have many teams of agile app development teams using Cloud Foundry installed on a few large Kubernetes clusters, whilst folks with more unique requirements and greater infrastructure knowledge have their own Kubernetes clusters and access them without Cloud Foundry.

## Tutorial Roadmap

The rest of this tutorial takes a deeper look at how the two technologies are similar, differ, and ultimately compliment each other. Next, we look at the common cloud abstractions before looking at deploying applications. Lastly, we discuss options for running Cloud Foundry on or with Kubernetes.
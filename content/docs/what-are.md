+++
title = "What are Cloud Foundry and Kubernetes?"
weight = "5"
+++

Whilst we've covered "What is Cloud Foundry" in another tutorial, and there are plenty of great resources explaining Kubernetes, let's summarise them in context of each other.

* **Cloud Foundry** is a Platform-as-a-Service that makes it easy to deploy and operate _applications_.
* **Kubernetes** is an open-source system for automating deployment, scaling, and management of _containerized programs_. It features an extensible API that makes it a great way to declaratively manage a wide variety of things.

## Cloud Foundry and Applications

Cloud Foundry is designed to make it easy to take application source code and get (and keep!) it running in the cloud. It has lots of built-in automation that works out-of-the-box to make this as effortless as possible. It is often used to run code built by the same organisation that is deploying it, rather than off-the-shelf software.

Cloud Foundry is not a general-purpose compute platform. You wouldn't run databases _on_ Cloud Foundry, instead you'd connect your apps on Cloud Foundry to databases running elsewhere.

## Kubernetes and Containers

Kubernetes can run anything in an OCI container image. It can run the same applications as Cloud Foundry (albeit with extra configuration), but is also suitable for running stateful databases and other workloads.

## Running Cloud Foundry on Kubernetes

Because Kubernetes is designed to be as generic as possible, you can run the components of Cloud Foundry itself _on_ Kubernetes!

+++
title = "What is Project Eirini?"
weight = "80"
+++

Unless you're particularly interested in _how_ Cloud Foundry and Kubernetes create containers, you don't really need to know what Project Eirini is or how it works. It's an implementation detail that is only really of concern to people operating Cloud Foundry.

**Eirini** is a Cloud Foundry component that **runs applications as Kubernetes pods**, as opposed to using the Diego scheduler.

By leveraging Kubernetes as a container scheduler, Cloud Foundry maintainers do not need to spend so much time investing in Diego, and operators have a simpler system to debug.

The Cloud Foundry Foundation has [a page explaining Project Eirini](https://www.cloudfoundry.org/project-eirini/), and Chip Childers has also recorded a brief overview:

<div class="video-container" style="position: relative; padding-bottom: 56.25%; height: 0;">
<iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;" src="https://www.youtube.com/embed/wT1ZImIYkrs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## Some History

**Cloud Foundry pre-dates Kubernetes by five years**, perhaps more depending on how you define the first version of CF. This means that Cloud Foundry had to find ways of running and isolating applications from each other, _before_ some of the Linux kernel features that enable modern containers were implemented.

Over the last ten years, **Cloud Foundry has used a variety of schedulers** for running apps. Let's look at those in chronological order.

### 1. DEAs

Early versions of **Cloud Foundry pre-dated containers**, so some home-grown terminology was used. Because container images hadn't really been invented, the tarball that made up a runnable application was called a "droplet" - because that's what clouds are made of! The component that unpacked these tarballs and executed processes was thus called a Droplet Execution Agent, or DEA.

The DEA and associated components had scaling limitations, and as Cloud Foundry started handling bigger and bigger production workloads, a new implementation was needed.

### 2. Diego

As with much of Cloud Foundry the DEA was written in Ruby, and as time went on more components were being written in Golang (Go).

The DEA was to be replaced with a more scalable architecture, this time implemented in Go. This component was named Diego, as it was a replacement for the DEA written in Go, and the DEA was _going_. Cloud Foundry teams have often picked questionable puns for component names!

At the time that Cloud Foundry made the switch from DEAs to Diego, Kubernetes was a nascent and volatile project. Diego was thus a necessary next step whilst Kubernetes matured.

Diego has been the standard way of running Cloud Foundry for the last few years, and has proven to be extremely scalable. **Diego will continue to be used and supported** in situations where deploying Cloud Foundry on Kubernetes is not suitable.

### 3. Eirini

**Eirini replaces the Diego** scheduler, and instead **uses Kubernetes to run apps**.

When an app is pushed to Cloud Foundry, Eirini talks to Kubernetes and instructs it to create a numbers of pods (via a StatefulSet, for the curious).

In KubeCF the Kubernetes that Eirini talks to is almost always the same Kubernetes that KubeCF is deployed to.

It is also possible to have Cloud Foundry deployed to VMs with BOSH, and configure Eirini so that apps are run on Kubernetes, whilst the rest of Cloud Foundry runs on VMs. This is a less-common use case, and has mainly been used as a transitory solution.

**Eirini is less mature** than Diego, and at the time of writing **Windows-based applications are not supported**. Be sure to check the [latest release notes](https://github.com/cloudfoundry-incubator/eirini-release/releases) to see which features have been implemented.

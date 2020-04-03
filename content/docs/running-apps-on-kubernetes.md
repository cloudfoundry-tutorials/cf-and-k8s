+++
title = "Running apps on Kubernetes"
weight = "30"
+++

In earlier sections we looked at the conceptual steps to get and keep an app running, and also what those look like when using Cloud Foundry. In this section we'll look at the same steps when using Kubernetes directly, _without_ Cloud Foundry.

One stark difference is that, in comparison to Cloud Foundry's one-or-two opinionated workflows, Kubernetes offers many ways to achieve a given outcome. It would be impractical to list all possible approaches here, so we'll focus on a naive implementation, and additionally what would be likely in an enterprise environment.

## Getting it running

### Write the application

Kubernetes can run **any application that is packaged as an OCI container image**. This means all app code needs to be built into an image before it can be deployed (see below).

Cloud Foundry allows the pushing of Docker image-based apps to be disabled, to make it less likely that unauthorised, external code is run on the platform. This is clearly not possible on Kubernetes, so some consideration may be given to preventing untrusted code from reaching the platform.

Kubernetes operators may choose to use tools like [Notary](https://github.com/theupdateframework/notary) to **sign images** and thus **prove their provenance**. Kubernetes can be configured to pull images only from a private registry.

### Build a container image

Most folks will be familiar with using Docker to build an container image from a Dockerfile.

The simplest approach is for a developer to **run `docker build`** from their machine, and then **push the image to a registry** that the target Kubernetes can access.

In an enterprise environment, the building of images should be automated by a CI server, or automation running inside the Kubernetes cluster - the KNative project has elements which can automatically rebuild images when code changes are pushed.

Alternative approaches exist. If you wanted to leverage the power of buildpacks, you could use the [`pack` CLI](https://github.com/buildpacks/pack) to build an image from your source code.

### Enable HTTPS access

The Kubernetes cluster **needs to be configured to accept HTTP traffic** on given ports and direct them to the correct place.

In a local development environment, a developer might configure a NodePort Service. This causes all Kubernetes worker nodes to listen on a given port, and then direct traffic to the application pods. In order to use hostnames with this approach, a `hosts` file entry would be required. This approach isn't scalable, and is suitable only for trivial examples.

On a **hosted Kubernetes** service like GKE, **a developer could create a LoadBalancer Service**. This creates a load balancer in the underlying infrastructure, and then configures it to direct traffic to the correct port on Kubernetes workers. DNS then would be configured to point to the load balancer.

If there is no LoadBalancer Service available then the same approach could be taken, albeit provisioning the load balancer out-of-band.

The experience may be made more uniform and user-friendly through **use of an Ingress**. An Ingress comprises two components: a deployment of pods that actually handle HTTP traffic (such as NGiNX) and a controller which watches the Kubernetes API to see if new rules have been applied, or new pods deployed. The Ingress can be configured to direct traffic according to a number of strategies depending on your preference. An Ingress is functionally equivalent to the Cloud Foundry GoRouter.

### Connect to data services

The [Open Service Broker API](https://www.openservicebrokerapi.org/) and [Service Catalog](https://kubernetes.io/docs/concepts/extend-kubernetes/service-catalog/) projects make a Cloud Foundry-like workflow for data services entirely possible. However, this approach is less popular amongst Kubernetes users.

There is **no single approach to deploying data services in Kubernetes**. Developers may determine their own data service requirements, and deploy and operate their own databases, message brokers and so on. This **might be done via Helm** charts, or directly with Kubernetes YAML files.

Applications running in Kubernetes can be configured to know about data services via **Config Maps**, which inject data both via environment variables and flat files on disk. Sensitive data (e.g. **credentials) can be injected as Secrets**, which are encrypted at rest. Access and credential management will need to be considered.

Various data service vendors offer CRDs and controllers that implement the 'operator' pattern. These processes run inside the Kubernetes cluster, and watch for new configuration to be submitted to the cluster. They then take appropriate action, such as creating new users or schema. Each implementation is unique and will offer custom functionality.

Most IaaS providers offer Cloud Controller Managers that register CRDs and can then dynamically provision new IaaS services, such as hosted databases.

Operators may choose to run data services on behalf of developers, and issue connection details and credentials when requested.

## Keeping it running

### Scaling in response to load

Kubernetes has **built-in functionality** that makes it **very easy to automatically scale** the number of pods that are running. This can be done using a number of metrics, including custom user-defined ones.

The following command will make sure that there are at least two instances of `my-app`, and will add up to two more when CPU usage goes over 75%.

```terminal
kubectl autoscale deployment my-app --min=2 --max=4 --cpu-percent=75
```

Autoscaling in Kubernetes is **very flexible and configurable**.

### Recovery from failures

As long as they are created as ReplicaSets or Deployments, **Kubernetes will restart failing apps**.

As Kubernetes has more layers of abstraction, the explanation of what gets restarted in which circumstances is a little more complex.

Pods are made up of several collaborating processes in containers. If one of these processes fails, it will be restarted. If it continues to fail, Kubernetes will waiting an exponentially-increasing amount of time before restarting the process again, capped at five minutes. Unlike Cloud Foundry, in Kubernetes this behaviour can be disabled.

Pods themselves will be restarted as long as they were deployed via a ReplicaSet (and so also a Deployment, which itself creates ReplicaSets). If a pod is created in isolation, it will not be recreated upon deletion, or upon worker node failure.

### Logging

Kubernetes allows logs to be retrieved via the `kubectl logs` command.

**Logs can only be followed for one particular pod**, or app instance. If you want to stream logs across all of your app instances, then additional software will be required. CLIs like [Stern](https://github.com/wercker/stern) are available, or a more comprehensive solution can be architected (likely involving something like [FluentD](https://www.fluentd.org/)) and an external indexer, as described below.

`kubectl` can retrieve historic logs from a number of different pods by using the `--selector` flag - it's only following logs that doesn't work across many instances.

```terminal
$ kubectl logs --selector run=my-app
# Returns recent logs for all pods with the label

$ kubectl logs my-app-6db489d4b7-7dtj9 --follow
# Streams logs specific pod

$ kubectl logs --selector run=my-app --follow
error: only one of follow (-f) or selector (-l) is allowed
```

Just as with Cloud Foundry, logs in Kubernetes are ephemeral, and any important information should be pushed out to an external logging system such as ELK, Splunk or Papertrail, amongst others.

### Zero-downtime updates

Kubernetes' `Deployment` resource handles **zero-downtime, rolling upgrades** of apps.

Simply changing the Deployment's template specification (e.g. changing the image tag) will trigger a rollout automatically.

You can track the progress of a rolling upgrade:

```terminal
$ kubectl rollout status deployment.v1.apps/my-app
Waiting for deployment "my-app" rollout to finish: 2 of 5 updated replicas are available...
Waiting for deployment "my-app" rollout to finish: 3 of 5 updated replicas are available...
Waiting for deployment "my-app" rollout to finish: 4 of 5 updated replicas are available...
deployment "my-app" successfully rolled out
```

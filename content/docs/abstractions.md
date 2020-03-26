+++
title = "Abstraction Layers"
weight = "10"
+++

Workloads can be deployed to the cloud at varying levels of abstraction.

High levels of abstraction provide simplicity at the cost of control; low levels of abstraction offer lots of control, but are more complicated.

Generally speaking, low levels of abstraction tend to be older technologies. Newer technologies have built upon these to present simpler, more productive interfaces.

## What are the relevant layers of abstraction?

Going from low levels (more complicated, more control) to high (less complicated, less control), we have:

* Virtual machines (VMs)
* Containers
* Apps
* Functions

|                | Abstraction | Productivity | Efficiency | Start Speed | Restrictiveness |
|----------------|-------------|--------------|------------|-------------|-----------------|
| **VMs**        | Low         | Low          | Low        | Slow        | Low             |
| **Containers** | ↑           | ↑            | ↑          | ↑           | ↑               |
| **Apps**       | ↓           | ↓            | ↓          | ↓           | ↓               |
| **Functions**  | High        | High         | High       | Fast        | High            |

### Virtual Machines (VMs)

VMs are distinct operating system instances that share the same physical hardware. Access to this physical hardware is provided by a special type of operating system called a hypervisor, which ensures that the VMs are isolated, so that they cannot access each other's data, and also enforces limits upon the VMs so that they cannot use more of the physical resources than they have been allocated.

VMs are easy for traditional IT teams to adopt, as they can be treated very similarly to physical servers. VMs require a lot of configuration before that can run workloads: a guest operating system must be chosen, virtual networks must be configured, software must be installed, and applications deployed on the VM. All these must then be kept running and up-to-date.

Examples of hypervisor include KVM, ESXI, Xen and others.

Hypervisors alone are time-consuming and cumbersome to manage. What happens if a physical host fails? How do VMs get restarted on healthy hosts? How do we decide where to place VMs in the first place? Infrastructure-as-a-Service (IaaS) systems offer solutions to these problems, allowing operators to create VMs without having to worry about the underlying physical hardware.

Popular IaaS offerings that manage VMs across many hypervisors include VMware vSphere, OpenStack Nova, AWS EC2, Google Cloud Compute Engine, and Azure VMs.

### Containers

Containers are processes just like any other program, run by an operating system, but with additional restrictions placed upon them.

The restrictions either limit what that process can 'see' (such as other processes, network devices, parts of the filesystem) or what the process can use (RAM, shares of the CPU, disk space). Unlike a VM, all containers share the same Linux kernel and are running within the same operating system.

The files that a container can see and access are often packaged as a container image - essentially a tarball containing a layered filesystem. Docker pioneered this approach, and so often people refer to them as 'Docker containers', although there is now an [open standard for container images](https://github.com/opencontainers/image-spec).

In order to run containers on a single machine, one needs a program called a container runtime. These are used for starting, stopping and managing containers on a single machine. Examples include Docker, ContainerD, and Garden.

Containers running on a single machine are not very useful for production workloads - what happens if a machine running the containers fails? How do we decide where to run new containers?

Container schedulers manage containers across a number of machines, allocating workloads appropriately. Examples include Docker Swarm, Kubernetes' kube-scheduler, Cloud Foundry's Diego, Apache Mesos and more.

### Apps

An app is a compiled executable binary, or a set of files that a runtime (ie Ruby, Java) can execute. Apps typically implement the custom business logic that provides value to an organisation: eCommerce stores, microservices, batch processing systems, and so on.

Apps are normally packaged with the libraries that they depend on. Golang compiles all dependencies into a single binary, Ruby uses vendored gems, and Java applications can be packaged with all their dependent .jar files.

Notice that all of these dependencies are at the language level - they are not things that may need to be installed on the operating system that the apps will eventually be run on. Ruby apps will need a specific version of Ruby to be installed; Java apps a specific version of the Java Runtime Environment. Apps might also need specific 'native' libraries such as OpenSSL to be installed on the operating system too.

### Functions

Many apps contain functionality that need only execute when an event happens, such as a HTTP request is made, or a message is delivered to a queue. This functionality can be modelled as a function: for a given input, some processing is done and an output is given.

Rather than writing systems as one unit of code in the form of an application, instead they can be decomposed into individual functions. This enforces decoupling of code, and can be much more resource-efficient as processes are only running when they're actually needed.

Functions must be written for a Functions-as-a-Service platform, such as OpenFaaS, OpenWhisk, Riff, Knative and more.




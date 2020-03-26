# cf-and-k8s

In this tutorial, we discuss the roles of Cloud Foundry and Kubernetes in development organizations. We discuss how Cloud Foundry and Kubernetes can be used together.


Background

CloudNative abstraction layers
- functions
- apps
- containers
- vms

size, startup time, coupling increaeses lower down
abstraction, flexibility, distribution increase higher up
more to manage lower down

## Similarities

* Both can run apps in containers
* Both can run apps from Docker images
* Both can mean that your app developers don't have to know which IaaS is being used
* Kubernetes can do everything that CF does - you just need to choose, configure and maintain all the components yourself
* CF cannot do everything that Kubernetes does - it is more opinionated

## Differences

* CF is Playmobil doll's house; Kubernetes is a box of Lego. KubeCF is Lego house kit?
* CF has two personas: operator and developer. Kubernetes present the same experience to both
* CF offers operators more control over what is running in the platform
* Kubernetes is very extensible, CF is not
* Kubernetes offers a lower level of control (ie scheduling based on presence of GPUs)


How do I get an app running?
- building the app (not a low code platform)
- building containers
- ingress
- service bindings

What are my responsibilities once it is running?
- scaling
- recovery
- logging
- services
- app updates (zero downtime)

app & service separation (12 factor)





What is Cloud Foundry?
- how do I get it running on CF
	- buildpacks
	- docker
	- ingress

What is Kubernetes?
- how do I get an app running
	- build container
		- flexibility
		- resposibility
		- maintenance of the image
	- push container to registry
- kubectl run
  - deploys a pod (containers that can't be distributed)
	- replica set (group of pods)
	- deployment (versions replica sets)
- service ingress
- closer to the infrastructure
- greater control
- greater responsibility
	- updates
	- approved runtimes, etc


- Container scheduler
- A platform for building platforms (kelsey hightower tweet)

- K8s still is susceptible to nosiy neighbors

Workloads on each
CF
	- Stateless apps
	- Web & workers
K8s
	- stateful
	- Non-http workloads (other than workers)
	- Containerinzing legacy apps that are not stateless workers/web apps

- opinions in CF lead to massive developer and CI/CD efficiency gains
- opinions are not valid for all situations
- cf push is the right abstraction for most enterprise developers


- KubeCF
 - smaller footprint/cost CF instances
 - in development
 - undergoing work to get to scale
 - transparent to apps and users

- Eirini
	- uses k8s as the container scheduler in place of a system called Diego
	- currently no windows
	- schedules apps natively on k8s
	- cf push. No managing pods/ingress
	- transparent to apps and users

Using Cloud Foundry with Kubernetes
- Kibosh
 - Services deployed to K8s using Helm


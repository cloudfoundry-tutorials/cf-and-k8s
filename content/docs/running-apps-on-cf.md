+++
title = "Running apps on Cloud Foundry"
weight = "20"
+++

Having already looked at the abstract steps that are required to get and keep an app running, let's look in more detail at what that experience looks like when using Cloud Foundry.

It's important to note that these steps are the same regardless of whether your Cloud Foundry is deployed on VMs, or deployed on Kubernetes.

## Getting it running

### Write the application

Most Cloud Foundry users deploy apps that have been written in-house. Whilst Cloud Foundry has automation to make running applications from source code easy, it also supports running Docker images if you need to run some off-the-shelf software. This can be disabled if you want tighter control over what code is running in the platform.

### Build a container image

To go from app source code to running container, one single command is required:

```terminal
$ cf push php-test
```

This command is run from a directory with a copy of the source code. Whilst there are many options that can be specified either in-line or in a YAML manifest file, Cloud Foundry will assume that your app speaks HTTP and will configure sensible defaults.

The `cf` CLI will zip up the local code, and send it into Cloud Foundry.

```terminal
Comparing local files to remote cache...
Packaging files to upload...
Uploading files...
 331 B / 331 B [========================================================================================================================================================] 100.00% 1s
```

In a process called staging, **Cloud Foundry creates a reusable container image** from your app code. It does this using buildpacks, which can intelligently detect the type of app you've pushed, and build a container image with all the things it needs (such as the Ruby runtime for a Ruby app). The source code is saved inside Cloud Foundry, so that if an operator needs to rebuild the image, they can do so without the involvement of app developers.

Here we can see the staging process getting the ordered list of buildpacks, and trying each of them to see if they can handle the PHP app that has been pushed. You can skip this process by _telling_ Cloud Foundry which buildpack to use.

```terminal
Waiting for API to complete processing files...

Staging app and tracing logs...
   Downloading web_config_transform_buildpack...
   Downloading staticfile_buildpack...
   Downloading java_buildpack...
   Downloading ruby_buildpack...
   Downloading dotnet_core_buildpack...
   Downloaded ruby_buildpack
   Downloading nodejs_buildpack...
   Downloaded java_buildpack
   Downloading go_buildpack...
   Downloaded staticfile_buildpack
   Downloading python_buildpack...
   Downloaded web_config_transform_buildpack
   Downloading php_buildpack...
   Downloaded go_buildpack
   Downloading binary_buildpack...
   Downloaded dotnet_core_buildpack
   Downloading dotnet_core_buildpack_beta...
   Downloaded binary_buildpack
   Downloaded nodejs_buildpack
   Downloaded dotnet_core_buildpack_beta
   Downloaded php_buildpack
   Downloaded python_buildpack
```

There are lots of buildpacks that are distributed with Cloud Foundry, you can add community-developed buildpacks, or even write your own!

Next we see the buildpack add dependencies like Apache HTTPD to the container image:

```terminal
   Cell 21c1c7c0-5944-49ac-8f05-880462a6fa58 creating container for instance f1cc25bc-cf14-4c1f-8de6-ce55e8582fe3
   Cell 21c1c7c0-5944-49ac-8f05-880462a6fa58 successfully created container for instance f1cc25bc-cf14-4c1f-8de6-ce55e8582fe3
   Downloading app package...
   Downloaded app package (331B)
   -------> Buildpack version 4.4.10
   Installing HTTPD
   HTTPD 2.4.41
   Downloaded [file:///tmp/buildpacks/38181493fba64ed4771437cb09aabc7c/dependencies/https___buildpacks.cloudfoundry.org_dependencies_httpd_httpd-2.4.41-linux-x64-cflinuxfs3-14955ebb.tgz] to [/tmp]
   Installing PHP
   [...]
   Exit status 0
   Uploading droplet, build artifacts cache...
   Uploading droplet...
   Uploading build artifacts cache...
   Uploaded build artifacts cache (1.2K)
   Uploaded droplet (83M)
   Uploading complete
```

After building the container image, Cloud Foundry will assume that you want your app to be running, and will start one or more instances of it.

```terminal
Waiting for app to start...

name:              php-test
requested state:   started
routes:            php-test.cfapps.io
last uploaded:     Mon 30 Mar 16:04:22 BST 2020
stack:             cflinuxfs3
buildpacks:        php 4.4.10

type:            web
instances:       1/1
memory usage:    1024M
start command:   $HOME/.bp/bin/start
     state     since                  cpu    memory     disk           details
#0   running   2020-03-30T15:04:39Z   0.0%   4M of 1G   248.2M of 1G
```

### Enable HTTPS access

Cloud Foundry configures HTTPS ingress by default - you have to take extra steps to _not_ get HTTPS traffic routed to your app.

Operators will have configured Cloud Foundry to know which domains have DNS records pointing to it. These are then available to app developers to use as HTTP routes for their apps.

When the app was first pushed Cloud Foundry used the app's name as part of the URL for the app.

```terminal
Waiting for app to start...

name:              php-test
requested state:   started
routes:            php-test.cfapps.io
```

You can specify one or more URLs (routes in Cloud Foundry parlance), or let Cloud Foundry pick one at random. This flexible route manipulation can be used to orchestrate complex zero-downtime-deployment workflows.

Operators need to ensure that DNS records have been created that point to a load balancer, which in turn directs traffic to the Cloud Foundry instance. Currently HTTP ingress is handled by a component called GoRouter, but this is being swapped out for Istio.

### Connect to data services

It is explained elsewhere in this guide that it would be quite unusual to run persistent data services (databases) _in_ Cloud Foundry. Instead these are run elsewhere, and a Service Broker offers programmatic automated provisioning of these data services. Operators choose which Service Brokers to tell their Cloud Foundry about, meaning that they can maintain control of approved and supported services.

The details of provisioned services are provided to apps running in Cloud Foundry via an environment variable. This means that your application needs to know how to interpret this environment variable: thankfully, there are libraries available in many common languages that can do exactly that.

* `cf marketplace` shows all the services that developers can automatically create
* `cf create-service` creates a service instance, e.g. a MySQL database
* `cf bind-service` tells the platform that an app will be accessing a service, prompting the Service Broker to generate credentials, which Cloud Foundry then provides to the app

If you do not want to automate provisioning of services (e.g. schema in a manually-configured Oracle cluster) then you can tell Cloud Foundry about User Provided Service Instances. In this case a Cloud Foundry user runs a command that stores the service's details in Cloud Foundry, meaning that an application can be provided these details via an environment variable in exactly the same way as if it had be automatically created. In this way apps don't need to know if their service was created by automation, or by a human.

## Keeping it running

### Scaling in response to load

Cloud Foundry does not offer auto-scaling 'out of the box', and instead a top-level [App Autoscaler](https://github.com/cloudfoundry/app-autoscaler) project is provided which can be deployed by Cloud Foundry operators. Public Cloud Foundry platforms like Pivotal Web Services offer App Autoscaler as a marketplace service.

Users can also scale apps both horizontally (add more instances) and vertically (add more RAM) via the command line with the `cf scale` command.

### Recovery from failures

Cloud Foundry will restart unhealthy apps up to 200 times in an exponential back-off before concluding that they're irrecoverably broken.

Users can choose from three types of health check:

1. Is the process running?
1. Is the process accepting TCP connections on a given port?
1. Is the app returning HTTP 200 responses at a given path?

Unlike Kubernetes, Cloud Foundry does not have a separate 'readiness' check. If an app is healthy, then Cloud Foundry considers it fit to serve traffic.

### Logging

Cloud Foundry makes it very easy to see logs from all application instances at once with the `cf logs` command. This can either tail logs, or show recent log output.

Cloud Foundry automatically adds certain log entries to the log stream for an app. Whenever a HTTP request to the app flows through Cloud Foundry's GoRouter (which is equivalent to a Kubernetes HTTP ingress), a log entry is added showing the size, response code and processing time. Additionally events such as app crashes and restart are added to the log stream.

In the following output we can see log messages for _all_ instances of our app, along with other messages from Cloud Foundry (marked `CELL`) and the GoRouter which directs HTTP traffic to the app (marked `RTR`):

```terminal
$ cf logs php-test --recent

2020-04-03T12:59:23.80+0100 [CELL/0] OUT Container became healthy
2020-04-03T13:00:18.46+0100 [RTR/3] OUT php-test.cfapps.io - [2020-04-03T12:00:18.451294340Z] "GET / HTTP/1.1" 200 0 21 "-" "curl/7.64.1" "10.10.66.172:25616" "10.10.149.78:61090" x_forwarded_for:"146.199.157.1, 10.10.66.172" x_forwarded_proto:"http" vcap_request_id:"14db7c46-3630-4b8b-7078-f1d76cd00644" response_time:0.012676 gorouter_time:0.000103 app_id:"442f2b57-836b-47ba-9eb3-faa007253361" app_index:"0" x_b3_traceid:"1fc51556aa0458c1" x_b3_spanid:"1fc51556aa0458c1" x_b3_parentspanid:"-" b3:"1fc51556aa0458c1-1fc51556aa0458c1"
2020-04-03T13:00:18.48+0100 [APP/PROC/WEB/0] OUT 12:00:18 Hello, world!
```

App logs in Cloud Foundry are entirely in-memory, reducing the amount of disk I/O required. If logs need to be persisted for operability and regulatory reasons, Cloud Foundry can be configured to send logs to an external system like ELK or Splunk.

### Zero-downtime updates

Cloud Foundry offers several ways to achieve zero-downtime updates of applications.

The newest and simplest is to use the latest version 7 of the `cf` CLI, which offers the following:

```terminal
$ cf push --strategy rolling
```

If more control is required, Cloud Foundry has always offered access to the primitives to make a zero-downtime deployment via the mapping of HTTP routes. Whilst this has the advantage of allowing users to create more bespoke workflows, in most cases these are not necessary.

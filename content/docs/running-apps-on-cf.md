+++
title = "Running apps on Cloud Foundry"
weight = "20"
+++

Having already looked at the abstract steps that are required to get and keep an app running, let's look in more detail at what that experience looks like when using Cloud Foundry.

It's important to note that these steps are the same regardless of whether your Cloud Foundry is deployed on VMs, or deployed on Kubernetes.

## Getting it running

### Write the application

Most Cloud Foundry users deploy apps that have been written in-house. Whilst Cloud Foundry has automation to make running applications from source code easy, it also supports running Docker images if you need to run some off-the-shelf software.

The example output in this guide was gained by pushing the [US Government's PHP 'Hello World' example app](https://github.com/18F/cf-hello-worlds/tree/master/php). The manifest was deleted, to show what Cloud Foundry will do when provided the bear minimum information.

### Build a container image

To go from app source code to running container, one single command is required:

```terminal
$ cf push my-app
```

This command is run from a directory with a copy of the source code. Whilst there are many options that can be specified either in-line or in a YAML manifest file, Cloud Foundry will assume that your app speaks HTTP and will configure sensible defaults.

The `cf` CLI will zip up the local code, and send it into Cloud Foundry.

```terminal
Comparing local files to remote cache...
Packaging files to upload...
Uploading files...
 331 B / 331 B [========================================================================================================================================================] 100.00% 1s
```

In a process called staging, Cloud Foundry creates a reusable container image from your app code. It does this using buildpacks, which can intelligently detect the type of app you've pushed, and build a container image with all the things it needs (such as the Ruby runtime for a Ruby app). The source code is saved inside Cloud Foundry, so that if an operator needs to rebuild the image, they can do so without the involvement of app developers.

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

### Connect to data services

## Keeping it running

### Scaling in response to load

### Recovery from failures

### Logging

### Zero-downtime updates

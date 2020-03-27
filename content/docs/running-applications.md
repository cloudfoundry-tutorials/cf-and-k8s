+++
title = "What is involved in running an application?"
weight = "10"
+++

In this section we'll look at what things need to happen in order to go from some source code (or compiled code in the case of Java applications) to a running application in the cloud.

These steps are equally true of both Cloud Foundry and Kubernetes. Later on we'll look at the concrete actions you as a user of each system will need to take to make these happen.

First, in order to get a web app running, we'll need to

* Write the application
* Build a container image
* Enable HTTPS access
* Connect to data services

Once it's up and running, we need to consider how to operate it too:

* Scaling in response to load
* Recovery from failures
* Logging
* Zero-downtime updates

## Getting it running

### Write the application

We'll assume that you write your own applications, and that you don't just want to deploy other people's software.

The source code will likely live in a code versioning system (CVS) like Git or Subversion. Whilst hopefully developers will write and run unit tests from their local workstations, they will often want to try out new versions of the app before committing their changes to CVS.

### Build a container image

The app code needs to be combined with files it depends on in order to be run. This might include a runtime like Ruby or Java, or third-party libraries like OpenSSL.

The image will also contain instructions on which executable should be started, and any arguments thereof.

A platform like Cloud Foundry or Kubernetes can then make use of this image to start new instances of the application.

### Enable HTTPS access

Most enterprise applications communicate via HTTPS - they are either web applications or RESTful microservices.

The platform running the application will need to provide a way for HTTPS clients to make requests of it, and preferably with a friendly human-memorable URL.

In most cases we'll want the _platform_ to present a TLS certificate, to save application developers from worrying about this.

### Connect to data services

Applications should not have hardcoded, 'baked in' knowledge of which data services (e.g. databases or message queues) that they need to connect to. This stops them from being portable between environments, and is [one of the 12 Factors](https://12factor.net/backing-services).

Before the app can connect to any such services, they need to exist! Ideally app developers would be able to create these in a self-service manner, so that they do not have to wait for service tickets to be satisfied by an operations team.

The platform should provide an application with the means to find out the details of such services, such as their location, and credentials for accessing them.

## Keeping it running

### Scaling in response to load

When our app becomes popular, we'd like to make sure we can add extra instances of it in order to deal with increased traffic. Sometimes we'd like to do this explicitly on-demand, and other times it'll be appropriate to scale the app automatically based on a metric.

### Recovery from failures

Nobody likes getting paged at 4am. We'd like our platform to automatically restart app instances if they fail, and then we can look into the problems in more detail during office hours.

### Logging

In order to debug problems, we'd like to be able to get the logs from all application instances at once. It'd also be helpful if we could see log messages from the platform that relate to our application, like access logs of when users made requests to it.

As well as reading those logs interactively, we'd also like to store them somewhere durable where they can be indexed and searched. In many industries, this may be a regulatory requirement.

### Zero-downtime updates

It's rare for an app to be deployed only once! We'll need to add enhancements, fix bugs, and apply security fixes. When we do, we'd like to be able to do so without our users noticing.

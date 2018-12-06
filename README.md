# Introduction to Kubernetes

# What is a container?
Containers are made possible by a set of facilities in the Linux Kernel that allow lightweight partitioning of a host operating system into isolated spaces—containers—where applications can safely run.

_Credit:_ [Pantheon - Why Containers?](https://pantheon.io/platform/why-containers)

Containers offer a logical packaging mechanism in which applications can be abstracted from the environment in which they actually run. 

Instead of virtualizing the hardware stack as with the virtual machines approach, containers virtualize at the operating system level, with multiple containers running atop the OS kernel directly. This means that containers are far more lightweight: they share the OS kernel, start much faster, and use a fraction of the memory compared to booting an entire OS.

_Credit:_ [cloud.google.com - Containers 101](https://cloud.google.com/containers/)

## Benefits of containers?
* Extremely fast provisioning. Containers can be quickly started, stopped and relocated.
* Each appliction's runtime environment and dependencies are defined by the developer and stored under source control.  
* IT operations teams focus on deployment and management without bothering with application details such as specific software versions and configurations specific to the app.
* Consistent environment as an application moves from development through to production.
_Credit:_ [cloud.google.com - Containers 101](https://cloud.google.com/containers/)

## Downsides of containers?
* Containers do not run at bare metal speed.  More suited to microservice style applications that can benefit from horizontally scale.
* Not all applications suit the architecture of microservies.
* Applications with a graphical frontend are not well suited to containers.  X11 forwarding is a clunky workaround. 


## What are the features of Kubernetes?
* Command line interface with `kubectl`
* Easy rollout of versioned deployments 
* Auto scaling
* Persistent Storage
* Role-based Access Control

## Key concepts of Kubernetes
* Container Registry
* Pod
* Deployment
* Service
* Persistent Storage
* Auto-scaling of the cluster nodes
* Auto-scaling of the pods

## How to get started?
### Sign up with GCP
Sign up with Google Cloud Platform.

You will hopefully have been awarded USD$300 of GCP credit which should hopefully last you a good amount of time.

### Do a hello-world tutorial
Follow the [GCP's Hello Kubernetes Lab](https://codelabs.developers.google.com/codelabs/cloud-hello-kubernetes/).

Following this lab you will have `gcloud` and `kubectl` installed on your laptop.

You will then have a basic usable Kubernetes Dev environment.

### Dockerise your application
Write a `Dockerfile` that can be built into an image that runs your application.

Your application can be a rudimentary nodejs webserver (that's what I did) or a java application with a dependency on a Cassandra cluster (which is what we eventually created).

To get started, your docker image will eventually be pushed to your own personal docker registry in GCP.

_*todo*: Mention how your docker runtime can be associated with the registry in your GCP project_

To do this, you will first have to tag your image as follows:
   
    docker build -t gcr.io/[MY_GCP_PROJECT_NAME]/myapplication:v1 .
    
Now push your image to your private docker registry in GCP as follows:

    docker push gcr.io/[MY_GCP_PROJECT_NAME]/myapplication:v1
    
_todo: Add a link to Google's video on Docker Best Practices_

### Create a simple Kubernetes cluster in GCP
_todo_

### Associate your local dev environment with your new Kubernetes cluster
_todo: Mention gcloud and kubectl commands that set things up and explore the cluster_

### Create your deployments
_todo_

### Create your services
_todo_

### Test and Debug your application
_todo_

## Kubernetes Workflow
* Declarative approach to K8s cluster design using YAML files  
* `kubectl` command line interface
* Namespaces and Labels

## Small demo
DM Payload Viewer and a single-node Cassandra instance are running in Google Cloud Platform.
Both services are deployed into Google Kubernetes Engine, GCP's managed Kubernetes Platform.
Both DM Payload Viewer and Cassandra each run in their own container, with each container running in its own Pod.

```
             +-------+                                       +-------+
             |Service|                                       |Service|
             +-------+                                       +-------+
                 |                                               |
                 |                                               |
                 |                                               |
                 |                                               |
+----------------------------------+            +----------------------------------+
|Deployment                        |            |Deployment                        |
|                                  |            |                                  |
|    +------------------------+    |            |    +------------------------+    |
|    |Pod                     |    |            |    |Pod                     |    |
|    |                        |    |            |    |                        |    |
|    | +--------------------+ |    |            |    | +--------------------+ |    |
|    | |Cassandra Container | |    |            |    | |Payload Viewer      | |    |
|    | |                    | |    |            |    | |Container           | |    |
|    | |                    | |    |            |    | |                    | |    |
|    | +--------------------+ |    |            |    | +--------------------+ |    |
|    |                        |    |            |    |                        |    |
|    +------------------------+    |            |    +------------------------+    |
|                                  |            |                                  |
+----------------------------------+            +----------------------------------+
```


## Some great resources for getting started
* [The Illustrated Children's Guide to Kubernetes](https://www.youtube.com/watch?v=4ht22ReBjno)
* [A Cloud Guru - Kubernetes Deep Dive](https://acloud.guru/learn/kubernetes-deep-dive)
* [GCP YouTube Channel - Kubernetes Best Practices playlist](https://www.youtube.com/watch?v=wGz_cbtCiEA&list=PLIivdWyY5sqL3xfXz5xJvwzFW_tlQB_GB)
* [GCP's Hello Kubernetes Lab](https://codelabs.developers.google.com/codelabs/cloud-hello-kubernetes/)
* [Hello Minikube Tutorial](https://kubernetes.io/docs/tutorials/hello-minikube/#create-your-node-js-application)
* [Kubectl Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)


# Todo
* Start sketching out the "How To Get Started" section.
* Tidy up the yaml files
* Add a step by step guide for recreating our POC in a new GCP project
* Make sure the dockerfiles are available at some point if there is no IP in those files.
* Mention the fact that we didn't explore RBAC.

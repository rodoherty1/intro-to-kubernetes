# Introduction to Kubernetes

## What did we do?

A java application and a single-node Cassandra instance are running in Google Kubernetes Engine.

The java app and Cassandra each run in their own container, with each container running in its own Pod.

The Cassandra deployment mounts a volume which is provisioned by Google Cloud Platform.

This ensures that when Cassandra pods are re-provisioned, the data is not lost.

```
             +-------+                                       +-------+
             |Service|                                       |Service|
             +-------+                                       +-------+
                 |                                               |
                 |                                               |
                 |                                               |
                 |                                               |
+----------------+-----------------+            +----------------+-----------------+
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
+----------------+-----------------+            +----------------------------------+
                 |
                 |
                 v
           +-----+----+
           |          |
           |    2GB   |
           |          |
           +----------+
```

## Some great resources for getting started
* [The Illustrated Children's Guide to Kubernetes](https://www.youtube.com/watch?v=4ht22ReBjno)
* [A Cloud Guru - Kubernetes Deep Dive](https://acloud.guru/learn/kubernetes-deep-dive)
* [GCP YouTube Channel - Kubernetes Best Practices playlist](https://www.youtube.com/watch?v=wGz_cbtCiEA&list=PLIivdWyY5sqL3xfXz5xJvwzFW_tlQB_GB)
* [GCP's Hello Kubernetes Lab](https://codelabs.developers.google.com/codelabs/cloud-hello-kubernetes/)
* [Hello Minikube Tutorial](https://kubernetes.io/docs/tutorials/hello-minikube/#create-your-node-js-application)
* [Kubectl Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## What is a container?
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

## Short list of some basic Kubernetes concepts
* Command line interface with `kubectl` which wraps and extends the familiar `docker` command.
* Declaritive workflow - Developer describes desired cluster state in yaml files and publish these to the K8s API server. 
* Container Registry
* Pod
* Deployment
* Service
* Persistent Storage
* Auto-scaling of the cluster nodes
* Auto-scaling of the pods
* Role-based Access Control

## Getting Started With Kubernetes?
In brief:

* Sign up with GCP
* Do the hello-world tutorial
* Select an application and Dockerise it
* Create your Kubernetes Cluster
* Set up your local dev environment
* Deploy your application
* Expose your application
* Test and Debug your application

... and now, in detail ... 

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

First you should confirm that your docker image runs correctly on your local machine.

Next, configure your docker runtime so that it can push images to the docker container registry in your GCP project.

    gcloud auth configure-docker

Now tag your image with a name that GCP will accept.

    docker tag myapplication:v1 gcr.io/[MY_GCP_PROJECT_NAME]/myapplication:v1

Alternatively you could just build your image with a specific name.
   
    docker build -t gcr.io/[MY_GCP_PROJECT_NAME]/myapplication:v1 .
    
Now push your image to your private docker registry in GCP as follows:

    docker push gcr.io/[MY_GCP_PROJECT_NAME]/myapplication:v1
    
As a side note, check out this [8 minute video](https://www.youtube.com/watch?v=wGz_cbtCiEA) from the official GCP YouTube channel about how to keep your docker images small.

### Create a simple Kubernetes cluster in GCP
Open the [GCP Console](https://cloud.google.com/) -> _Kubernetes Clusters_ and click _Create Cluster_.

Select a _Standard Cluster_.  This will create a 3 node cluster with modest hardware performance.

### Associate your local dev environment with your new Kubernetes cluster
When your cluster has been provisioned, click on the small _connect_ button.

You will see a shell command like the following which you should run in your local dev machine.

    gcloud container clusters get-credentials <YOUR_CLUSTER_NAME> --zone us-central1-a --project <YOUR_GCP_PROJECT_NAME>
    
Hopefully, the following commands also work.

    kubectl get pods
    kubectl get services
    kubectl get deployments
    kubectl get pvc
    kubectl get pv 
    
All of the above will show no resources because all you have done is provision some VMs and started an empty Kubernetes cluster.  We have not depoyed any services ```kubectl``` has nothing to report. 
    
### Create a Kubernetes Deployment
Your application is represented by the docker images you created earlier.

When launched, your application will run in a docker container.

In Kubernetes, it is standard practice to launch each docker container inside its own Kubernetes Pod.  

You can generally replace the word "pod" with "container" and accurately understand the concept.

_Credit:_ [coreos.com](https://coreos.com/kubernetes/docs/latest/pods.html) 

To launch your container in a pod, you must describe to Kubernetes how you want your application to run.  

This description is expressed in ```yaml``` and is called a `deployment descriptor`.  See [payloadviewer_deployment.yaml](https://github.com/rodoherty1/intro-to-kubernetes/blob/master/payloadviewer_deployment.yaml).

You launch the payloadviewer pod by publishing ```payloadviewer_deployment.yaml``` to your cluster as follows:

    > kubectl apply -f payloadviewer_deployment.yaml

You may now view your new pod.

    > kubectl get pods
    
    
### Create a Kubernetes Service

Pods are expected to be created and destroyed reasonably frequently.

Auto-scaling rules may create or destroy pods or you may have a busy continuous deployment pipeline.

When a pod is destroyed and a new one takes its place, it has a new hostname and ip address. 

To help establish a reliable presence in your cluster, you may create a service for your replica-set of pods.

You may assign your service a user-friendly hostname and its IP address will remain the same throughout its service to your chosen replica-set.

It is expected that other pods in your cluster will locate each other by interacting with ```service``` objects in your cluster.

Here is a [sample service](https://github.com/rodoherty1/intro-to-kubernetes/blob/master/payloadviewer_service.yaml) which is launched in the same way that we launched the ```deployment``` in the previous section.

    > kubectl apply -f payloadviewer_service.yaml
   
When the service has been created, you will see that it has an external IP Address.

    > kubectl get service payloadviewer

This is the IP address that you will use to communicate with your application.


### Attach persistent storage to your Pod

In  Google Cloud Platform, it is very simple to attach persistent storage to your pods.

Two steps are required:

#### 1) Create a Persistent Storage Claim object on your Kubernetes cluster.  

A Persistent Storage Claim (PVC) is a request for storage.  For example: I would like a 20GB disk.

Here is a sample [PVC](https://github.com/rodoherty1/intro-to-kubernetes/blob/master/cassandra-pvc.yaml).

#### 2) Attach the PVC to your Deployment

The next step is to mount the provision disk on your pods as follows.

```
      volumes:
      - name: cassandra-persistent-storage
        persistentVolumeClaim:
          claimName: cassandra-pv-claim
```

See the bottom of [cassandra_deployment.yaml](https://github.com/rodoherty1/intro-to-kubernetes/blob/master/cassandra_deployment.yaml) to see how the above snippet is used.

With this snippet in place, every pod that is created as part of this deployment, will mount this disk.

The disk will remain as pods are created and destroyed.  Your data is safe!

### Test and Debug your application

The above steps show how to launch a docker container in a pod and expose it to the world using a Kubernetes Service.

If, however, you wish to debug it, there are some options.

    > kubectl get pods
    > kubectl exec -it [MY_POD_NAME] /bin/bash    # You are now ssh'd into your container and you may debug as you wish. 
    > kubectl logs payloadviewer                  # This will show some logs that your application may have produce while it was running.




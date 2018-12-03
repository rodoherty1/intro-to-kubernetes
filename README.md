# Introduction to Kubernetes

# What is a container?
> Containers are made possible by a set of facilities in the Linux Kernel that allow lightweight partitioning of a host operating system into isolated spaces—containers—where applications can safely run. - [Pantheon - Why Containers?](https://pantheon.io/platform/why-containers)

> Containers offer a logical packaging mechanism in which applications can be abstracted from the environment in which they actually run. 

> Instead of virtualizing the hardware stack as with the virtual machines approach, containers virtualize at the operating system level, with multiple containers running atop the OS kernel directly. This means that containers are far more lightweight: they share the OS kernel, start much faster, and use a fraction of the memory compared to booting an entire OS. - [cloud.google.com - Containers 101](https://cloud.google.com/containers/)

## Benefits of containers?
* Extremely fast provisioning.  Containers can be quickly started, stopped and rebalanced.

* Containerization provides a clean separation of concerns.  Developers focus on their application logic and dependencies, while IT operations teams can focus on deployment and management without bothering with application details such as specific software versions and configurations specific to the app. - [cloud.google.com - Containers 101](https://cloud.google.com/containers/)

## Is there a problem with containers?

## What are the features of Kubernetes?
* Smooth deployments and version control 
* Auto scaling
* Persistent Storage
* Role-based Access Control

## How to get started?

## Kubernetes Workflow
* YAML Files 
* `kubectl` command line interface
running in
## Small demo
* Payload Viewer and Cassandra are running in separate pods on Google Cloud Platform

## Some great resources for getting started
* [A Cloud Guru - Kubernetes Deep Dive](https://acloud.guru/learn/kubernetes-deep-dive)
* [Hello Node Minikube](https://codelabs.developers.google.com/codelabs/cloud-hello-kubernetes/)
* [Hello Minikube Tutorial](https://kubernetes.io/docs/tutorials/hello-minikube/#create-your-node-js-application)
* [Kubectl Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)


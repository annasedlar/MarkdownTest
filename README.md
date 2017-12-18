---
layout: post
title:  "KubeCon Review - Containers, Kubernetes, Orchestration, woah!"
date:   2017-12-11 15:00:00
author: "@annasedlar"
tags:
- containers
- orchestration
- deployment
- devops
- cloud 
- conferences
- kubernetes
- go
- infrastructure

---


> "Cloud native infrastructure is more than servers, network, and storage in the cloud—it is as much about operational hygiene as it is about elasticity and scalability” 
-- RedHat

# What is Kubernetes and Why Should BNR Care? 
Kubernetes is a container orchestration system built by teams at Google. The poroject now lives under the Cloud Native Computing Foundation, a vendor-neutral space with strong community support. According to the tech news, it’s one of the fastest growing projects of all time! You may have heard of Docker. While these two projects exists very closely, they are not the same. Docker is used to build containers themselves, it is considered a container runtime (Docker is also similar to Virtual Machines, in that both are abstraction layers running atop a machine. Docker containers, however are smaller and quicker to spin up than VMs.) Containers are a means of bundling or packaging a software application with it's dependencies that can then be run by a system that supports this format (ie. The Docker Engine). Docker is all about managing apps within an individual machine. Kubernetes is the platform that can manage, scale, monitor, and configure these containers.

Kubernetes was designed to operationalize containerized applications. Using Kubernetes, containers will run under a single service in what they introduced as Pods. Kubernetes is often referred to as a Container Orchestration Environment (COE). Think fleets of containers across multiple hosts. Docker recently released a new project, Docker Swarm which is it's own COE and addresses these functions similarly to Kubernetes. COEs manage the containers when running multiple instances of a containerized application. A COE’s simplest function is to launch an application and ensure that application is running. If a given Container instance fails, Kubernetes (or Docker Swarm) would recognize this and spin up another container (or rather, another Pod in Kubernetes case). Kubernetes can also be configured to scale the application up or down in response to demand.

Kubernetes is definitely more Devops than Dev work. In fact, it's more Ops than Devops. I was certainly in the 1% of least experienced at this conference of 4300. It was fasinating to witness the excitement in what normally is the slower, more stable world of Ops and I can certainly see the benefits of managing apps in containers in the cloud. Cloud-native - the concept of the cloud being the default deployment environment for apps (versus in proprietary data centers that must be monitored and controlled by a company's own employees). I left exhausted and excited to learn more.

What value does this hold for Big Nerd Ranch? This was on my mind throughout the conference and to be honest, I'd love to hear the thoughts from other, more experienced nerds on this topic. From what I understand, our client work is usually a code hand-off and the client is responsible for deploying the application where and how they see fit. And for our in-house apps, I have only seen us use Heroku, which seems perfectly sufficient for hosting our small apps. I imagine if we were a product team, Kubernetes would definitely be more relevant. 

# Nitty Gritty Components of Kubernetes 
### ([Thanks to this article for concise definitions](https://blog.giantswarm.io/understanding-basic-kubernetes-concepts-i-introduction-to-pods-labels-replicas/))
#### Pods
The smallest deployable unit of computing that can be created and managed in Kubernetes. Pods *can* contain one single container, but they aren't limited to just one. All containers in a pod run as if they would have been running on a single host in pre-container world. They share a set of Linux namespaces and do not run isolated from each other. This results in them sharing an IP address and port space, and being able to find each other over localhost or communicate over the IPC namespace. Further, all containers in a pod have access to shared volumes, that is they can mount and work on the same volumes if needed. A YAML (Yet Another Markup Language) file is used to define a pod. Below is an example pod written in YAML:
```apiVersion: v1
kind: Pod
metadata:
 name: nginx-pod
 labels:
  app: nginx
spec:
 containers:
 - name: nginx
  image: nginx:1.7.9
```

#### ReplicaSets
A pod by itself is ephemeral/'mortal' and won’t be rescheduled if the node it is running on goes down. ReplicaSets ensure that a specific number of pod instances (or replicas) are running at any given time. If you want your pod to stay alive you make sure you have an according replica set specifying at least one replica for that pod. The ReplicaSet then takes care of (re)scheduling your instances for you.
A ReplicaSet can not only manage a single pod but also a group of different pods selected based on a common label. This enables a replica set to for example scale all pods that together compose the frontend of an application together without having to have identical ReplicaSets for each pod in the frontend.

#### Deployments
A controller that provides declarative updates for Pods and ReplicaSets by changing the state of them at a controlled rate. Used for creating new ReplicaSets or removing existing Deployments. 
The folloring Deployment creates a ReplicaSet to bring up three nginx Pods: 
```apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

#### Services
A service is a grouping of pods that are running on the cluster. Services are "cheap" and you can have many services within the cluster of Pods. Kubernetes services can efficiently power a microservice architecture. Services provide important features that are standardized across the cluster: load-balancing, service discovery between applications, health checks and features to support zero-downtime application deployments. The example below targets TCP port 80 on any Pod with the `run: my-nginx` label, and expose it similar to a REST API. 
```apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx
```

# Gimme Some Context - The Big Dog$ in the Container Game:
### Or at least those that were represented at KubeCon
* Google
* Google Cloud Platform
* Heptio
* IBM
* Microsoft - Azure
* Amazon - AWS
* Docker
* RedHat
* CoreOS
* Tigera
* Mezosphere
* DataDog
* Sysdig
* WeaveWorks
* Mirantis
* huawei
* Meteor
* Dynatrace


# Notable Talks
* Guinevere's
* Justin ___
* ??? 
* ?? 

# Take Aways
### Helpful Resources
* [Kubernetes Docs (incl. tutorial)](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
* [Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
* [Kubernetes Bootcamp](https://kubernetesbootcamp.github.io/kubernetes-bootcamp/)
* [ATL Kubernetes Meetup](https://www.meetup.com/Kubernetes-Atlanta-Meetup)
* [Kubernetes Mentoring Initiative](https://github.com/kubernetes/community/tree/master/mentoring)
* [Kubernetes.io](https://kubernetes.io/)
* [Cloud Native Computing Foundation](https://www.cncf.io/)
* [KubeCon 2018 Copenhagen (May 2-4) Scholarship Opportunity](http://events.linuxfoundation.org/events/kubecon-and-cloudnativecon-europe/attend/scholarship-opportunities) 
* [Kubernetes youtube live 'Office Hours' Thursdays 1pm EST](https://www.youtube.com/c/KubernetesCommunity/live)
* [Join Kubernetes Slack Channel](http://slack.k8s.io/)
* [Linux Foundation Events](https://events.linuxfoundation.org/events/kubecon-cloudnativecon-europe-2018/)

### People to Follow
* [Kelsey Hightower](https://twitter.com/kelseyhightower)
* [Michelle Noorali](https://twitter.com/michellenoorali)
* [Chen Goldberg](https://twitter.com/GoldbergChen)
* [Clayton Coleman](https://twitter.com/smarterclayton)
* [Amy Chen](https://twitter.com/TheAmyCode)
* [Sarah Novotny](https://twitter.com/sarahnovotny)
* [Jessie Frazelle](https://twitter.com/jessfraz)
* [Jorge Castro](https://twitter.com/castrojo)

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
*** NOTE: Currently in the process of being edited! *** 

> "Cloud native infrastructure is more than servers, network, and storage in the cloud—it is as much about operational hygiene as it is about elasticity and scalability” 
-- RedHat

# What is Kubernetes and Why Should BNR Care? 
Kubernetes is a container orchestration system built by teams at Google. You may have heard of Docker. While these two projects exists very closely, they are not the same. Docker is used to build containers themselves, it is considered a container runtime (Docker is also similar to Virtual Machines, in that both are abstraction layers running atop a machine. Docker containers, however are smaller and quicker to spin up than VMs.) Containers are a means of bundling or packaging a software application with it's dependencies that can then be run by a system that supports this format (ie. The Docker Engine). Docker is all about managing apps within an individual machine. Kubernetes is the platform that can manage, scale, monitor, and configure these containers.

Kubernetes was designed to operationalize containerized applications. Using Kubernetes, containers will run under a single service in what they introduced as Pods. Kubernetes is often referred to as a Container Orchestration Environment (COE). Think fleets of containers across multiple hosts. Docker recently released a new project, Docker Swarm which is it's own COE and addresses these functions similarly to Kubernetes. COEs manage the containers when running multiple instances of a containerized application. A COE’s simplest function is to launch an application and ensure that application is running. If a given Container instance fails, Kubernetes (or Docker Swarm) would recognize this and spin up another container (or rather, another Pod in Kubernetes case). Kubernetes can also be configured to scale the application up or down in response to demand.

Kubernetes is definitely more Devops than Dev work. In fact, it's more Ops than Devops. I was certainly in the 1% of least experienced at this conference of 4300. It was fasinating to witness the excitement in what normally is the slower, more stable world of Ops and I can certainly see the benefits of managing apps in containers in the cloud. Cloud-native - the concept of the cloud being the default deployment environment for apps (versus in proprietary data centers that must be monitored and controlled by a company's own employees). I left exhausted and excited to learn more.

What value does this hold for Big Nerd Ranch? This was on my mind throughout the conference and to be honest, I'd love to hear the thoughts from other, more experienced nerds on this topic. From what I understand, our client work is usually a code hand-off and the client is responsible for deploying the application where and how they see fit. And for our in-house apps, I have only seen us use Heroku, which seems perfectly sufficient for hosting our small apps. I imagine if we were a product team, Kubernetes would definitely be more relevant. 

# Nitty Gritty Components of Kubernetes

# Gimme Some Context - The Big Dogs in the Container Game:
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
* Dynatrace


# Notable Talks
* Guinevere's
* ??? 
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


Kubernetes 101 w/ Justin ____? - kubelets ~= docker. A single standalone unit that has one job (more options when in a cluster but you can have just a standalone kubelet on a server). Yaml on discs. Runs that container and that's his job.
etcd - database running in the cloud. Keeps track of each kubelet node with labels and pods (vic is running a django pod that's scheduled)
To talk between databases + pods, need API server. This is the only service that can talk to the database.
Containers make replica sets - how many pods we want.
Controller Manager - does a lot. Looks at replica sets and keeps track of them, reconsile state - matches replicas with how many we specified that we want in the database.

If something goes down, kubelet re-runs whatever is assigned to it. It has one job!

Services have labels, what labels the service itself wants to match, pods.
Ie: service: Ruby, Labels: Gem, Match Labels: Red, Pods:

If you lose a node, you can't change the state of the cluster. If etcd is down, you can't add a service.

Tour of Distributed Systems w/in Kubernetes: Bo Ingram
Pod = 1+ containers that share a unique IP address
Deployment = manage pods. Use replica sets to ensure # of pods is correct. Replica sets will create our pods, then a scheduler will ______ ?
Kubernetes is distributed

Master Kubernetes Components: etcd, API server, controllers, scheduler
etcd -> (via rest calls) -> API server -> controllers diff states -> scheduling unscheduled pods
Node Components: -kubelet, kube-proxy, container runtime
kube-poxy: maintains networking

etcd = data store. distributed key/value store. quora: min. number of healthy nodes.
CAP Theorum- consistency, availability, partition tolerance.
etcd = willing to sacrified availablity to achieve consistency + partition tolerance.

Raft = consensus algo for managing replicated log. State management. Elect raft leader. Three states: Leader, follower, candidate. One leader/term. Leader sends heartbeat messages. If no heartbeat, election time via rpc!

Check out: Raft Paper

Replica set has a selector to select labels to manage. Deployment controller manages the whole deployment process of your app.

Scheduler is constantly querying etcd and ____ looking for unscheduled pods to assign them to a node.

kubectl ->  How we submit our deployment. kubctl creates deployment, replica set creates 3 sets, scheduler schedules them, kubelet will run scheduled pods.

2017-12-10 10:07 Sharing KubeCon Experience in a NerdNote + NST
1st: learn how BNR normally does clients' deployments.

   - pods, etcd, scheduler, kubectl ...
   - differences between VM and container solutions
   - discuss specific sessions
   - books that'll leave at BNR bookcase ..


+++
title = "My take on Kubernetes: how and why?"
date = "2019-07-12"
author = "Rafael Moreira"
cover = "kubernetes.png"
images = ["posts/kubernetes-how-and-why/kubernetes_og.png"]
tags = [ "kubernetes", "k8s", "containerization" ]
description = "See what is my opinion on Kubernetes and how I share my experience on why you should use it and how you should use it." 
+++

## A bit of history

[Kubernetes](https://kubernetes.io/), commonly known as K8s, "is an open-source system for automating deployment, scaling, and management of containerized applications". Quotes aside, Kubernetes was release on 2014 by Google, as a free and opensource solution for orchestration on the cloud.

At the time, containerization was not something strange, since [Docker](https://www.docker.com/) had been around for 1 year, more or less. Docker brought out Docker-Swarm, a technology that was simple in it's concept: bring multiple machines together under one big roof, and have them work together, sharing resources and delivering computing to end-users.

On the other hand, Red Hat was offering a solution called [Openshift](https://www.redhat.com/en/technologies/cloud-computing/openshift), an also open-source and managed Kubernetes solution for enterprise-grade workloads. As you can see, at the time, containerization on the cloud was growing.

## Why containerization? And why not virtualization?

The main idea behind containerization is the ability to isolate workloads inside a system, making them independent from the host it is running on. This is important because it allows for easier management of services and improves security (more on this later).

The biggest alternative for containerization is virtualization. Whilst in containerization, usually, the engine runs on the host and shares its kernel for modules and basic Linux functionalities, in Virtualization each instance emulates its own Kernel, using the virtualization technology referring to each CPU architecture (AMD or Intel, for example).

Alibaba Cloud has a very succinct and simple post on Containerization vs Virtualization, you can see it here: [What Are the Differences Between Containers and VMs?](https://www.alibabacloud.com/knowledge/difference-between-container-and-virtual-machine).

In a nutshell, containerization allows you to port your application to any system capable of running the containerization engine in question, since you build an image that is run in a container. Each image has multiple layers that define the configuration of the final image and will result on your service/application.

So is containerization better then virtualization? Yes and no. Until this day, virtualization has advantages over containerization since, for instance, virtualization emulates a kernel and that comes with advantages for security, but containerization is faster, lighter and easier to manage.

Needing an hypervisor is also a downside to virtualization, since using Docker Engine will save you RAM and instantiate your containers way faster. Ever noticed how, when you boot up your Linux desktop there are a bunch of messages on the screen (may differ from one distribution to another)? Those are _pre-flight_ checks and service startups. Docker does not need that for each container, so the start process is simpler and faster.

## Kubernetes and containerization, what's the relation?

After Docker, we had containers. Each container was a service. But now what?

The main idea behind using multiple commonly called "servers" is simple: sharing the burden of the workloads and making sure your service is always available in the event of a failure on one of the servers.

But this was not enough. Sure you could design your cloud infrastructure in such a way that multiple servers communicated between each other and shared information about load status, health status in a more traditional way with the [LEMP Stack](https://lemp.io/), the [XAMPP Stack](https://www.apachefriends.org/) or any other stack you desired. For accessing multiple servers and perform updates on your software, service or maintenance on the OS itself you could use [Ansible](https://www.ansible.com/), for example (these are just some examples, there is an World of custom and very intelligent solutions to explore).

This is where Kubernetes reigns. Now that we know that we can have fast and light services running in each OS, in multiple containers, we want to orchestrate them, control them and maintain them. K8s does exactly that in a very customizable way.

Kubernetes allows you to [bring your own container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) and if the options are not enough, just invent one and make it compatible, it's open-source so you can go hardcore with it and the [`kubelet`](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) will handle the management of your pods, their containers and the basic k8s services (that run themselves in pods as well, in each node, incredible right?).

K8s also has one very important aspect: it connects tightly to the Operating System it is running on. There is no surprise that the main Linux Distributions run some sort of version of the Linux Kernel and that every version that exists out there shares basic functions because that's how and why Linux works. K8s takes advantage of these aspects and let's you control aspects such as: runtime users (GID and GUID), RAM requests and limits for each container, privilege escalation (this one is very powerful and prevents so much damage in case of an attack), amongst an extensive list of possible configurations.

So, the main idea with Kubernetes is: bring all the children under one roof. You define the container specification, K8s will ensure they are healthy, running and that they don't misbehave, no matter how many nodes (aka "servers") you have, giving you important information about the node itself in the process (RAM, Disk Usage, etc).

## Kubernetes is Cloud-Vendor agnostic, but there's a catch...

K8s was designed to be Cloud Vendor agnostic. This means that a volume is a volume, no matter where you are: AWS, Azure, Google Cloud, etc. This means that the core functionalities should be the same wherever you are running your workloads on.

In fact, you can run Kubernetes on-premises and there are a lot of tools that allow you to scale your Kubernetes Clusters up and down as you wish, in a _bring your own hardware_ kind of party. The aforementioned tool Ansible allows for [some solutions](https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/), but you have other solutions such as [Kubespray](https://github.com/kubernetes-sigs/kubespray) or [Talos Linux](https://www.talos.dev/)(one of my favorite projects so far regarding K8s, incredible work has been done by the Talos Team).

But there is a catch with this: how hard it is to manage your Kubernetes Clusters. For instance, if you have a small data-center, say 20 servers, you would still have the need to run an hypervisor to provision virtual machines to run K8s nodes on (this is one aspect virtualization is still needed and will be for a lot of time). Why? Because current AMD Threadripper CPUs have 64 cores and you want to share those cores between multiple virtual machines. One machine using 64 physical cores will do you no good besides the _flex_ that comes with it (please note that current Hypervisors are really fast and efficient, and technology improves daily).

Cloud-Vendors take this aspect to their advantage and offer you managed solutions, such as [AWS's EKS](https://aws.amazon.com/eks/) or [Google Cloud's Kubernetes Engine](https://cloud.google.com/kubernetes-engine). These are services that offer you a seamless Kubernetes experience, while maintaining a tight integration with the other available services from that vendor.

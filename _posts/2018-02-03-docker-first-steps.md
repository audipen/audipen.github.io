---
title: "First steps into the Docker world"
date:   2018-02-03 
categories: containers
tags: 
  - docker
---

Containers are really gaining popularity now and one can't afford to be oblivious to them anymore. Docker is the most popular tool out there that provides containerization. Here are some first steps into the Docker world.

 - Installation 
   ------------
   To install Docker on a Windows machine, download the installer from the [Docker website](https://docs.docker.com/docker-for-windows/install/). You can choose between a stable version and an experimental version. What you need to keep in mind is that ['Virtualization' must be enabled](https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization-must-be-enabled) in order for Docker to work.
   
 - Beginner Labs
   -------------
   Docker maintains a Github repo with [hands-on Labs](https://github.com/docker/labs) aimed at beginners and also some advanced security and networking labs.
   
 - Frequently used commands
   ------------------------
   Some commands you would use frequently in the labs are
	- **[docker container ls](https://docs.docker.com/engine/reference/commandline/container_ls/)** : List containers 
	- **[docker container run](https://docs.docker.com/engine/reference/commandline/container_run/)** : Starts a new container and runs a command in it
	- **[docker container stop container-name](https://docs.docker.com/engine/reference/commandline/container_stop/)** : Stops one or more running containers
	- **docker container stop $(dock container -q)** : During development it is quite common to want to stop all running containers. The 'docker container -q' command provides the preceding 'docker stop' command with a list of numeric IDs for all running containers.
	- **[docker container rm container-name](https://docs.docker.com/engine/reference/commandline/container_rm/)** : Remove one or more containers
	- **[docker images](https://docs.docker.com/engine/reference/commandline/images/)** : List images present locally
	- **[docker image build](https://docs.docker.com/engine/reference/commandline/image_build/)** : Build an image from a Dockerfile
	
	Note that most of the 'docker container' commands have older versions (for example, 'docker ps' is the same as 'docker container ps') which will probably be deprecated in the future. 

 - Docker on Windows
   -----------------
   There are a couple of things to keep in mind when it comes to Docker on Windows.You can have basically three types of containers on Windows
   - **_Linux containers_** : These run in a Linux Hyper-V VM running on Windows. All containers run within this single VM and share the Linux kernel, similar to what you would expect of Linux containers running on a Linux machine.
   - **_Windows containers_** : Native Windows containers run directly in the host where multiple containers share the host Windows kernel. 
   - **_Hyper-V containers_** : Docker on Windows offer something called as Hyper-V containers. These are a bit special.Each container runs in it's own lightweight Hyper-V VM. Yes,a VM is spun up for every container. This offers additional isolation if required and also the possibility to run different versions of the OS or even a combination of Windows and Linux containers.
	
   
   
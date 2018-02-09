---
title: "First steps into the Docker world"
date:   2018-02-03 
categories: containers
tags: 
  - docker
---

Containers are really gaining popularity now and one can't afford to be oblivious to Docker containers anymore. Here are some first steps into the Docker world.

 - Installation 
   ------------
   To install Docker on a Windows machine, download the installer from the [Docker website](https://docs.docker.com/docker-for-windows/install/). You can choose between a stable version and an experimental version. What you need to keep in mind is that ['Virtualization' must be enabled](https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization-must-be-enabled) in order for Docker to work.
   
 - Beginner Labs
   -------------
   Docker maintains a Github repo with [hands-on Labs](https://github.com/docker/labs) aimed at beginners and also some advanced security and networking labs.
   
 - Frequently used commands
   ------------------------
   Some commands you would use frequently in the labs are
	- [docker container ls](https://docs.docker.com/engine/reference/commandline/container_ls/) : List containers 
	- [docker container run](https://docs.docker.com/engine/reference/commandline/container_run/) : Starts a new container and runs a command in it
	- [docker container stop container-name](https://docs.docker.com/engine/reference/commandline/container_stop/) : Stops one or more running containers
	- [docker container rm container-name](https://docs.docker.com/engine/reference/commandline/container_rm/) : Remove one or more containers
	- [docker images](https://docs.docker.com/engine/reference/commandline/images/) : List images present locally
	- [docker image build](https://docs.docker.com/engine/reference/commandline/image_build/) : Build an image from a Dockerfile
	
	Note that most of the 'docker container' commands have older versions (for example, 'docker ps' is the same as 'docker container ps') which will probably be deprecated in the future. 
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
	- [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) : List containers 
	- [docker run](https://docs.docker.com/engine/reference/commandline/run/) : Starts a new container and runs a command in it
	- [docker stop container-name](https://docs.docker.com/engine/reference/commandline/stop/) : Stops one or more running containers
	- [docker rm container-name](https://docs.docker.com/engine/reference/commandline/rm/) : Remove one or more containers
	- [docker images](https://docs.docker.com/engine/reference/commandline/images/) : List images present locally
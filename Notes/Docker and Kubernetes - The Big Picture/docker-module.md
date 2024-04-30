![Docker Logo](/images/docker/docker-logo.png)

# Docker

This file contains notes for the "**Docker**" section of the [Docker and Kubernetes: The Big Picture](https://app.pluralsight.com/library/courses/docker-kubernetes-big-picture/table-of-contents) PluralSight Class by Nigel Poulton.

## Summary

- Docker & Containers are key to moving to modern cloud native microservices design.

- Docker is two things:
  1. Docker, Inc. the company: All about helping people to move to containers and provide and enterprise platform with enterprise support.

  2. Docker the technology: Apps in containers, 1. Write Code 2. Build a Docker Image 3. Push to a Registry 4. Start a Container.

- Learning Docker is easy, but it's more complex running it at scale. There are several tools to help, but two common tools are **Docker Swarm** & **Kubernetes** (k8s is most common).

## ----------------------- Deeper Explanation -----------------------

## Docker, Inc

- Docker, Inc. is a tech startup from San Francisco and it's the main sponsor behind the open-source container technology "Docker".

- Docker, Inc. started out named "**dotCloud**" that provided a developer platform on top of Amazon Web Services (AWS).

- That platform was not a sustainable business. They had already been using containers to base their tech around so in 2013, they decided to make their own container technology called "**Docker**" & gave it to the world.

- "**Docker**" is slang for a "dock worker". The Docker tech is a lot like a shipping port so this slang makes sense.

## Docker: The Technology

- **Containers are like fast, lightweight virtual machines, and Docker makes running our apps inside of containers really easy.**

- ### Community Edition (CE)

  - Open source

  - Lots of contributors

  - Quick release cycle

- ### Enterprise Edition (EE)

  - Slower release cycle

  - Additional features

  - Official support by Docker, Inc.

## Docker Demo

- In the cloud native microservices world, every single service (Web, API GW, Catalog, Cart, Checkout, etc.) is coded separately and each one lives in its own container.

- Sometimes one team is responsible for all of them, but sometimes you will have different teams responsible for different services.

- This means each service can be fixed, updated, etc. independent from the rest.

- ### Docker Workflow

1. Write Code 2. Build a Docker Image 3. Push to a Registry 4. Start a Container

![Docker Workflow](/images/docker/docker-workflow.png)

- Windows PowerShell Commands to build/run a Docker Image:

- `PS: C:\FOLDERPATH> docker image build -t DOCKERID/APPNAME:2 .`

- Class Example: `PS: C:\docker-demo> docker image build -t nigelpoulton/ctr-demo:2 .`

- ### Build Image Explanation

  - `docker` denotes this is a `docker` command.

  - `image` denotes this will be a Docker Image.

  - `build` denotes this needs to build that Docker Image.

  - `-t` denotes it is running with a tag.

  - `DOCKERID` denotes this is the user that is building the Docker Image. This is your Docker username.

  - `APPNAME` denotes this is the name of the Docker Image.

  - `:2` This is the tag of the Docker Image.

  - `.` this denotes it will build the image using all of the files in the current directory and below.

- Once this command runs, Docker takes the source code & doing the hard work to package it as a container/image (an image is like a stopped container).

- Once that is done, you use the following command to open the image: `PS: C:\FOLDERPATH> docker image ls DOCKERID/APPNAME:TAG`

- Class Example: `PS: C:\docker-demo> docker image ls nigelpoulton/ctr-demo:2`

- All is the same as Build Image except for no `build`, `-t`, or `.`.

- Instead you have `ls` which denotes you will be using a list of Docker Images. The list is the following path `

- Those commands finished step 2 of the workflow (**Build a Docker Image**)

- ### Step 3 - Push to Registry

  - Next, you push the image to a registry. You can have your own on-premises or private registries, or push to DockerHub.

  - `PS: C:\FOLDERPATH> docker image push DOCKERID/APPNAME:TAG`

  - Class Example: `PS: C:\docker-demo> docker image push nigelpoulton/ctr-demo:2`

  - DockerHub is really similar to GitHub, your Docker Image should show up shortly after pushing it to DH.

- ### Step 4 - Run Container

  - Next, you run the container with this command: `docker container run -d --name APPNAME -p PORT:PORT DOCKERUSER/REPO:TAG`

  - Class Example: `docker container run -d --name web -p 8080:8080 nigelpoulton/ctr-demo:2`

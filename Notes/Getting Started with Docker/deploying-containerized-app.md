![Docker Logo](/images/docker/docker-logo.png)

# Deploying a Containerized App

This file contains notes for the "**Deploying a Containerized App**" section of the [Getting Started with Docker](https://app.pluralsight.com/library/courses/docker-getting-started-2023/table-of-contents) PluralSight Class by Nigel Poulton.

## Summary

- Open Container Initiative (OCI) are the standards on how containers should be made: image spec, runtime spec, distribution spec (registries).

- An "**OCI Image**" is jargon for a **container image** or **docker image**, those three are all the same thing.

- **Docker Workflow:** 1. Write Code 2. Build a Docker Image 3. Push to a Registry 4. Start a Container

- Docker is language-agnostic which means it does not care which language you use to build your app, it supports ALL languages.

- A Docker Image is like a stopped container. A container is like a running image.

- A VM Template is like a stopped VM. A VM is a running instance of a VM template.

- These are similar to classes in Object-Oriented Programming (OOP): An OCI Image is like a class, and a running container is like an object made from that class.

- Images are buildtime constructs and containers are runtime constructs.

- If an app works on your laptop, it will work in production.

## ----------------------- Deeper Explanation -----------------------

### Docker Workflow&colon;

1. Write Code

2. Build a Docker Image

3. Push to a Registry

4. Start a Container

![Docker Workflow](/images/docker/docker-workflow.png)

## Containerizing an App

- Docker is language-agnostic which means it does not care which language you use to build your app, it supports ALL languages.

- Docker images have a file named "**Dockerfile**" which is a set of instructions on how to build the Docker image.

- If you reverse that, it means a container image is **app code** and all its **dependencies** all neatly packaged in a format that makes it easy to share and run.

- The file at `learn-docker-kl/Notes/"Docker and Kubernetes"/docker-module.md` has the commands for building/pushing the docker image.

- This is an example of a Dockerfile (**image below**):

    1. grabs the **node:current-alpine** image (**FROM**)

    2. set the metadata (**LABEL**)

    3. creates an image directory using the source code (**RUN**)

    4. copy the app code into #3's directory (**COPY**)

    5. set the working directory as #3's directory (**WORKDIR**)

    6. Install dependencies (**RUN**)

    7. Execute the image build command (**ENTRYPOINT**)

![Dockerfile Example](/images/docker/dockerfile-example.png)

## Hosting on a Registry

- The previous section built the docker image, but that image is "landlocked" on your local computer.

- You want to host the image to a centralized registries where it can be accessed from different environments.

- When you push an image to DockerHub or any other centralized registry, it defaults to the **OS/ARCH** that matches the computer you are on. This means the app will only run on computers that share the same **OS/ARCH**.

- To get around this, you can use the `docker buildx build` command to specify the platform(s) you want to run.

- Class Example: `docker buildx build --platform linux/arm64/v8,linux/amd64 --push --tag nigelpoulton/gsd:ctr2023 .`

- After you push your image, you need to pull up the registry and check the image is there & registered under the correct platform(s) you want it to have.

## Running a Containerized App

- When you build a docker image, it will initially be stored locally on your machine. You can run it locally, or run it via the registry.

- If you want to run it via a registry, it is good to delete the local image using this command: `docker image rm DOCKERID/FOLDERNAME:IMAGENAME`

- Class Example: `docker image rm nigelpoulton/gsd:ctr2023`

- To double check it was deleted, run this command: `docker image ls`

- The file at `learn-docker-kl/Notes/"Docker and Kubernetes"/containers-primer.md` has the commands for running the container.

- Class Example: `docker container run -d --name web -p 8000:8080 nigelpoulton/gsd:ctr2023`. This defaults to looking for the image on DockerHUB. If you want to use a different registry, put the URL in front of the image like so: `docker.io/nigelpoulton/gsd:ctr2023`. The `docker.io` designates DockerHUB is the desired registry.

- When you run that `docker container run` command, Docker will first look for a local image & if not found, it will look for one on the desired registry (or DockerHUB if none is specified) & will run it if found.

- You can check the container exists by running `docker container ls` and that will give you a list of all active containers.

- You can also check for the container by opening a web browser & going to the given localhost port (**localhost:8000**)

- A Docker Image is like a stopped container. A container is like a running image.

- A VM Template is like a stopped VM. A VM is a running instance of a VM template.

- These are similar to classes in Object-Oriented Programming (OOP): An OCI Image is like a class, and a running container is like an object made from that class.

- Images are buildtime constructs and containers are runtime constructs.

![Container Image VM Analogy](/images/docker/container-image-vm-analogy.png)

## Managing a Containerized App

- **Containerized App** is an application running inside a container.

- Containers are like fast, lightweight VMs.

- To stop a container, you run the command `docker container stop`, followed by the name or ID of the container, then refresh your page to confirm it can't connect.

- Class Example: `docker container stop web`

- To restart a container, you run the command `docker container start`, followed by the name or ID of the container, then refresh your page to confirm it can connect again.

- Class Example: `docker container start web`

- To delete a container, you first stop the container, then run the command `docker container rm`, followed by the name or ID of the container.

- Class Example: `docker container rm web`

- By default, you can only delete stopped containers, but you can run `docker container rm web -f` & that `-f` flag will force it to stop & delete the terminal at once.

- After each of these commands, you can run `docker container ls -a` to find the list of all containers and the status should update according to what commands you ran.

- If you delete a container, now you will get an error that it doesn't exist if you try to restart it.

- Typically running a container will detach it from the terminal and run independently, but you can use the following command to run with with a terminal: `docker container run -it --name test alpine sh`

- Doing this will switch your command line into the container so all commands you run are being executed on the container itself.

- Running the command `exit` will exit the container, but it also kills the container. If you want to exit but not kill the container, use the shortcut `Ctrl+P+Q` which exits the container back to your regular terminal while keeping the container active.

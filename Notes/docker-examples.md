![Docker Logo](/images/docker/docker-logo.png)

# Docker Example

Here are some commands to run Docker on your own. You might need to follow along with the notes (located inside `learn-docker-kl/notes`) to understand what each command does.

First, Install Docker on your system. Docker provides installation instructions for various operating systems on their [official website](https://docs.docker.com/engine/install/).

Next, create your own [Docker Hub Account](https://hub.docker.com/). This will give you a Docker ID which is your account username. Replace **DOCKERID** in the following commands with your own Docker ID.

Next, clone this repo wherever you want it on your computer: <https://github.com/nigelpoulton/gsd.git>

Once that is cloned, open Windows PowerShell or Command Prompt, then use the `cd` command to enter the `gsd/container` repository. Ex. `cd Desktop/gsd/container`. Then run the following commands for the Image Examples and Container Examples.

It is not required, but you can look through the files in the **gsd/container** folder to familiarize yourself with its **Dockerfile**.

## Docker Image Example&colon;

**Build Image**:

- `docker image build -t DOCKERID/gsd:ctr2023 .`

**Check Image**:

- `docker image ls`

**Push Image to Registry**:

- `docker image push DOCKERID/gsd:ctr2023`

**Add Multiple OS**:

- `docker buildx build --platform linux/arm64/v8,linux/amd64 --push --tag DOCKERID/gsd:ctr2023 .`

**Remove Image**:

- `docker image rm DOCKERID/gsd:ctr2023`

## `Docker Container Examples:&colon;`

**Run Container**:

- `docker container run -d --name web -p 8080:8080 DOCKERID/gsd:ctr2023`

**Check Container**:

- `docker container ls`

**Find App at Localhost**:

<http://localhost:8080/>

**Container Stop**:

- `docker container stop web`

**Check Container**:

- `docker container ls -a`

**Restart Container**:

- `docker container start web`

**Container Stop**:

- `docker container stop web`

**Remove Stopped Container**:

- `docker container rm web`

**Check Container**: (this should show no containers)

- `docker container ls -a`

**Restart Container (this should give an error stating no container exists)**:

- `docker container start web`

## Docker Compose Example&colon;

First, you need to use the `cd` command to enter the `gsd/compose` folder. Ex. If you're already in `gsd/container`, run `cd ../compose`. Then run the following commands for the **Compose Examples**. To run the **Compose Examples**, you will need to have already done the **Image Examples** and **Container Examples**.

I would recommend looking through the files in the `gsd/compose` folder to familiarise yourself with the **Dockerfile** & **compose.yaml** file.

**Build with Compose**:

- `docker compose up –detach`

**Check Container**:

- `docker container ls`

**End the app with Compose Down**:

- Make sure you add `-volumes` to the end. This will remove the volumes param, otherwise it will stay running even when the app is down.
- `docker compose down –volumes`

**Multipass Examples**:

MULTIPASS EXAMPLES IS STILL BEING WORKED ON.

![Docker Logo](/images/docker/docker-logo.png)

# Containers Primer

This file contains notes for the "**Containers: Primer**" section of the [Docker and Kubernetes: The Big Picture](https://app.pluralsight.com/library/courses/docker-kubernetes-big-picture/table-of-contents) PluralSight Class by Nigel Poulton.

## Summary

- In the "old days" you used one server for one app which had upfront/operating costs and wasted a lot of server potential.

- VMWare allowed multiple apps to run on one server. This was more efficient than single server/apps, but it was still slow because you had to run separate VMs & OS's for each app.

- Containers use very little resources compared to VMs so containers are faster, cheaper, and give us better utilization. They need no VM & only one OS for all apps.

- You can use Containers like VMs, for example Docker Inc. has a program called "**Modernize Traditional Apps**" where you shift old legacy apps into containers. This works, but it's like using a swiss army knife to only cut paper: sure it works, but you aren't utilizing the knife effectively and the rest of the tools are wasted.

- While containers are very useful and used in the majority, containers won't entirely replace VMs or even mainframes. Sometimes certain people prefer VMs/mainframes, sometimes it's way too much work to port over to containers, etc.

- Containers consist of a Docker Image which is a pre-packed application, or a VM template. It has everything you need to run an application wrapped up in a single bundle.

## ----------------------- Deeper Explanation -----------------------

## The Bad Old Days

- Applications run businesses.

- It is nigh impossible to distinguish between a business and the apps that power it.

- No apps, no business

- Apps run, for the most part, on servers

- In the early 2000s and earlier, you would run a single app on a single server

- Anytime you need to run a new app, you need to buy a whole separate server which comes with the initial purchase cost and the operating costs.

- It was hard to know exactly how big or fast a server had to be. IT erred on the side of caution and opted for big and fast in most cases.

- 99 times out of 100, they ended up with overpowered servers that only used maybe 5% - 10% of their full operating power.

## Hello VMWare

- The world changed for the better once VMWare came along.

- Instead of using one server for one app, you could use VMWare to run multiple apps on one server.

- This means when you need to run a new app, it's not an automatic purchase of a new server, you can just add the app to a server that has space.

- 99 times out of 100, you only buy a new server when your existing server(s) is/are too full.

## VMWarts

- A physical server has processors, memory, and disk space.

- You first set up a Hypervisor on the server which allows for virtual machines (VM) to be built.

- To run four apps for example, you create four VMs and each one of these VMs are a slice of the physical hardware.

- All VMs share portions of the resources made by the physical server, such as virtual RAM, virtual CPU(s), virtual memory card(s), etc.

- Each VM needs their own operating system (OS) which use lots of resources from the server just to run the OS, no apps yet.

- You may also need four separate operating system licenses, one for each VM which can get expensive as VMs stack up.

- All VMs/OS needs License costs, admin costs such as patching, updates, AV, & more.

## Containers Explained

- A big advantage of containers over VMs is that containers use only one single OS for all containers/apps, not a separate OS for each app.

- After the single OS is created, you create separate containers, one for each app. 1 : 1 ratio.

- Containers are smaller and more efficient than VMs.

- A VM is a virtual construct built to look and feel like a physical server.

- Containers allow you to trim all of the "fat" by removing the resource-hungry VMs & OSs which frees up resources and space for more apps/features.

- The VM/OS model is slow to start because it has to boot the VM, then boot the OS, then boot the app.

- The Container model is fast to start because the OS is already running & no VM is needed, so you only have to boot up the single app.

## Container Demo

- Docker can run on any type of server, such as a virtual machine, physical server (AKA bare metal server), laptop, etc.

- At the base level, Docker on Windows can run Windows apps and Docker on Linux can run Linux apps. At higher levels, you can run Linux apps on Windows.

- A Docker Image is a pre-packed application, or a VM template. It has everything you need to run an application wrapped up in a single bundle.

- This is the basic command to run a docker image container. There are more options you can add, but those are more advanced. `docker container run -d --name APPNAME -p PORT:PORT DOCKERID/REPO:TAG`

- Class Example: `docker container run -d --name web -p 8080:8080 nigelpoulton/ctr-demo:1`

- To access the Docker Image container after you run it, you need to know the server's IP address ( `12.345.678.012` ) & the exposed network port ( `8080` ). Example&: `12.345.678.012&:8080`

- To stop a Docker Image, you run this command&: `docker stop APPNAME`, then you refresh the browser & it should show the port is closed & not running. Class Example&: `docker stop web`

- You can restart the Docker Image quickly with this command&: `docker container start APPNAME`, then you refresh the browser & it should show the port is closed & not running. Class Example&: `docker container start web`

## Legacy/Monolithic Apps

- Legacy/Monolithic apps are old, large apps where everything the app does is baked into a single binary (fancy term for a single program).

- In monolithic app design, all app functionality such as a search bar, authorication, item stock check, store cart, cart checkout, etc, is stored in a single program.

- This is strong because it's all contained within one location, but it's bad because if you need to make any change, you are changing the entire massive program and you could easily break something. The only way to fix something is to bring the entire app down until you fix it, and it might take a long time, which is not ideal especially for a business.

- This is where **cloud native** and **microservices** are great because they break the program into small pieces. Any changes are made to only the related microservice(s) and it will update wherever that piece is used. Only the item being changed needs to be down for changes.

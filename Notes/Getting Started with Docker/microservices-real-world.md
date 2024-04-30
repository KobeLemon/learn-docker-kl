![Docker Logo](/images/docker/docker-logo.png)

# Deploying a Containerized App

This file contains notes for the "**Microservices and the Real World**" section of the [Getting Started with Docker](https://app.pluralsight.com/library/courses/docker-getting-started-2023/table-of-contents) PluralSight Class by Nigel Poulton.

## Cloud Native Microservices

- Old Monolithic Apps run the entire app on one single service which is strong because you only need to host one item, but it's weak because if you need to make a change, even a single line of code, you have to take down the entire app which completely halts business.

- In a Microservices design, every different feature (cart, checkout, search, profile, auth, etc.) will be hosted in their own separate containers.

- This makes it easy to make changes/fixes because only the affected container(s) will be down instead of everything down.

- Cloud Native is a category whereby an app/container has capabilities such as self-healing, rolling updates, and more. It doesn't mean cloud-based, as you can make cloud-native apps locally on your machine or in the cloud.

## Multi-container Apps

- You can build, push, & run containers all through the command lines, but that is clunky & you have to rewrite the commands every time. Instead of this, you can use a **declarative** approach.

- Declarative means to explain what the app should be like instead of saying all of the steps to make the app. You can do this by building a configuration file that has all of the command line commands, microservices, etc. stored & the platform (DockerHub, Docker Swarm, etc.) automatically uses that config file to build the app. This is called a **compose.yaml file**.

![Compose File Example](/images/docker/compose-file-example.jpg)

- When you have a **Dockerfile** & **compose.yaml** file in your app files, you can run the command `docker compose up --detach` to fully build the image & create/run the container. The `--detach` option runs the app independently of your terminal.

- When you want to shut down the app, you can run the command `docker compose down`. Volumes stick around in case they have important data so if you want to remove volumes, you add `--volumes` to the end of that command.

- This has all been run on the local machine. The next module (Docker Swarm Fundamentals) will explain how to use it in production.

## Docker Swarm Fundamentals

- ### Jargon&colon;

  - **Nodes**: Anything that runs the app. Ex. physical servers, virtual machines, cloud instances, etc.
  
  - **Cluster**(aka **Swarm**): A bunch of nodes with Docker installed & network connectivity.
  
  - **Multipass VM**: Lightweight VMs that can easily run on Windows, Mac, or Linux.
  
  - **Control Pane**: Intelligence of the cluster.(cluster store, scheduler, etc.)
  
  - **Manager**: A node that hosts a control plane.
  
  - **Worker**: A node that runs the business app(s).
  
  - **Split Brain**: When a network issue isolates half of the managers which caused them all to freeze.
  
  - **Read-Only Mode**(ROM): A mode where nodes can only be read & not changed.

- Docker has a Mode called **Swarm Mode** that lets you connect multiple Docker hosts into a secure cluster.

- When you build the Cluster/Swarm, every node has to be either a worker or a manager.

- In the real world, you want to run three or five managers for high availability. Three is the most commons, but the most important thing is that you have an odd number of managers.

- You need to run as many workers as your app requires.

- You need to run the nodes across availability zones so when one zone/path goes out, the node still has other zones/paths it can use. This avoids the entire node or even the entire cluster from shutting down.

- Docker is built so when a network issue occurs to isolate certain nodes, the majority side keeps running and the minority side goes into Read-Only-Mode until the issue is fixed.

- The reason for an odd number of managers: example: if you have four managers and a network issue occurs that isolates two of them, both sides know there used to be four but they can only see two. Neither side knows if they are the majority or minority, so they assume the worst (they are minority) & both sides go into ROM. If you have an odd number, one side will always have the majority so there will always be some nodes still running.

- The only time all nodes will go into ROM (or fully shut down) is if the entire network crashes. If it's bad enough to get to that point, it doesn't matter how the managers are set up because all will be down.

![Cluster Example](/images/docker/cluster-example.png)

## Building a Swarm

- Docker Desktop can only run one node at a time. If you want to run multiple nodes (99.99% of the time you will), then you either need to use [Play With Docker](https://www.docker.com/play-with-docker/) or [Multipass](https://multipass.run/install)

- Multipass allows you to spin up Ubuntu VMs to then run multiple nodes at once.

### How to Spin up multiple nodes&colon;

**Use Windows Powershell for all of these commands:**

1. This is the command to launch a docker node with Multipass. Replace the "`NODENAME`" name with whatever name you want: `multipass launch docker --name NODENAME`. Repeat this for all of the nodes you need (use different names for each).

    - To double check the node was created, run `multipass list` and it will show you all of the currently existing nodes.

    - To delete a node, run `multipass delete`.

    - Deleted nodes will stay around & you can recover a single node (`multipass recover NODENAME`), recover all nodes (`multipass recover --all`), or you can fully remove all deleted nodes which cannot be undone (`multipass purge`). You can't purge individual nodes, all deleted nodes will be purged.

2. Find node1's IP Address by running `multipass info node1`, then copy the "**IPv4**" address. If there are multiple IP addresses, copy the topmost one.

3. To enter the shell for a specific node, run `multipass shell NODENAME`.

4. To initialize the Docker Swarm, enter the shell for node1, then run `docker swarm init --advertise-addr IPADDRESS`. Replace "**IPADDRESS**" with your node's IP address. Initializing the Docker Swarm automatically makes that node the first manager.

5. To add new managers, run `docker swarm join-token manager`, then PowerShell will give you a new command: `docker swarm join --token SWMTKN-1-TOKEN IPADDRESS`. Copy that entire command.

6. Now you need to exit the current node by typing `exit`, then run `multipass shell NODENAME` on the next node you want to make a manager.

7. Then run the command `docker swarm join --token SWMTKN-1-TOKEN IPADDRESS`, then PowerShell will say "`This node joined a swarm as a manager.`". Repeat steps 5 & 6 for all other nodes you want to make into managers.

8. Run `docker node ls` to find the list of manager nodes. FYI, the asterisk (\*) in the image below denotes that is the current node open.

    ![Manager Nodes](/images/multipass/node-managers.png)

9. To add new workers, run `docker swarm join-token worker`, then PowerShell will give you a new command: `docker swarm join --token SWMTKN-1-TOKEN IPADDRESS`. Copy that entire command.

10. Go into the shell of node4, then run the command `docker swarm join --token SWMTKN-1-TOKEN IPADDRESS`, then PowerShell will say `This node joined a swarm as a worker.`. Exit node4's shell & repeat step 10 for node5.

11. Run `docker node ls` again to find the list of all nodes. The image below shows all of the nodes. 1-3 are managers & 4/5 are workers. When the "**Manager Status** has something in it, that node is a manager. When that field is blank, that node is a worker.

    ![All Nodes](/images/multipass/node-managers-and-workers.png)

12. You will want to run the command `docker node update --availability drain NODENAME` onto all of your managers to stop business apps from being scheduled onto the managers. This keeps the managers clean & puts the business apps onto the workers.

13. These commands below will build 5 multipass docker VM nodes, enter node1 to initialize the swarm, then make nodes 1-3 as managers & 4/5 as workers:

```console
multipass launch docker --name node1
multipass launch docker --name node2
multipass launch docker --name node3
multipass launch docker --name node4
multipass launch docker --name node5
multipass shell node1
docker swarm init --advertise-addr IPADDRESS
exit
multipass shell node2
docker swarm join --token SWMTKN-1-MANAGERTOKEN IPADDRESS
exit
multipass shell node3
docker swarm join --token SWMTKN-1-MANAGERTOKEN IPADDRESS
exit
multipass shell node4
docker swarm join --token SWMTKN-1-WORKERTOKEN IPADDRESS
multipass shell node5
docker swarm join --token SWMTKN-1-WORKERTOKEN IPADDRESS
exit
multipass shell node1
docker node update --availability drain node1
docker node update --availability drain node2
docker node update --availability drain node3
```

## Working with Docker Swarm

- This section is creating services imperatively, which means you write out each command line by line. This is nice because you're in control, but it is very slow & is prone to mistakes.

- Containers by themselves are not smart enough to scale themselves up/down or perform rollouts/rollbacks. They can pretty much only run the app. To do these things, you need something smarter than controls the container.

- You can use **Swarm Service Objects** to control containers.

- To create a Docker Service, run the command `docker service create --name SERVICENAME -p PORT:PORT -- replicas 3 DOCKERUSER/REPO:TAG`. You can only run this command in an initialized swarm and in a manager's shell.

- Class Example:  `docker service create --name web -p 8080:8080 -- replicas 3 nigelpoulton/gsd:web2023`

- ### Service Explanation&colon;
  
  - `docker service create` is the command that creates a Docker Service based on the data following that command.
  
  - `--name web` is the name of the service.
  
  - `-p 8080:8080` is the port where the service will run.
  
  - `--replicas 3` specifies the number of containers the service will deploy & manage. You can set the number to however many containers you need to run.
  
  - `nigelpoulton/gsd:web2023` is the image that the service will be based upon.

- After you run the `docker create service` command, run the command `docker service ls` to confirm the service was created.

- To find a list of all containers that were just created, run the command `docker service ps SERVICENAME`. The image below shows the three web service containers, the image they are based on, the node they are running on, the state they are supposed to be running, and the actual state they are running.

![Multipass Containers](/images/multipass/multipass-containers.png)

- If you want to find which specific containers are running on a specific node, you need to enter that node's shell & then run the command `docker container ls`.

- Docker Swarm has a nice feature where you can hit any node in the cluster (ours has 1-5) on the service's exposed port (ours has 8080) & you'll reach the containers and apps they are running. This means we can hit any of our five nodes & reach the app.

- To access a specific node, you open a web browser & in the address field, you enter the node's IP Address, followed by the port you get from `docker service ps SERVICENAME`. Example: `123.45.67.890:8080`.

- The site that opens shows the pod/container/host's name that serviced the request. If you refresh the page a few times, you will see that name change. This is called "**load-balancing**" where the load is shared across the different nodes so they all support each other.

- Running services this way allows the service to be scalable. If you want to scale up or down, you run the command `docker service scale SERVICENAME=SCALEAMOUNT`. Example: `docker service scale web=10` will build 10 total containers. If the service already has containers built, this command leaves those containers alone & only adds however many new containers it needs. Our example leaves the first 3 containers alone & creates 7 more containers.

- You can run `docker service ps web` again to verify all of the containers were created.

- If you enter a node's shell & you want to remove containers, you can run `docker container rm CONTAINERID` replace "**CONTAINERID** with the container ID(s, you can do multiple at once) that you want to remove. However, this won't fully remove the containers. Instead it will shutdown the ones you chose & will auto-spin-up new containers to replace them so the overall containers stay the same.

- This process of auto-spinning-up new containers is known as **self-healing** or **reconciliation**.

- If you want to actually remove the containers & scale down the total number of containers, you can run the `docker service scale SERVICENAME=SCALEAMOUNT` command with a lower number.

- If you want to remove the entire service itself, you can run the `docker service rm SERVICENAME` command with the desired service's name. Again, you can remove multiple services at once by adding each service's name to the end of that command, each separated by a space.

## Multi-Container Apps with Docker Swarm

- This section is creating services declaratively, which means you use a file that already has each command written out line by line to build the app. This is nice because it is very fast and consistent so you know the same commands are running every time.

- This is also nice because if you built an app a while ago, or you're looking at someone else's app or someone else is looking at your app, the reader can check the Dockerfile or Compose file & get up to speed on how it's built. It's like a summary for the app's build.

- The image below is an example of a Compose file from Nigel Poulton's `gsd/swarm` folder:

  ![Swarm Dockerfile](/images/docker/multi-app-compose-file-example.png)

- Before we can run that Compose file, we need to build the image. To do so, first we need to install Nigel Poulton's GitHub repository onto the multipass VM. This is all done on Windows PowerShell if using Windows.

- First, enter node1's shell (`multipass shell node1`), then check git is installed (`git --version`), then clone the repository (`git clone https://github.com/nigelpoulton/gsd.git`).

- Next, enter the gsd/swarm folder (`cd gsd/swarm`). Then build the image (`docker image build -t DOCKERUSER/REPO:TAG .`, ex. `docker image build -t nigelpoulton/gsd:swarm2023 .`). Then run `docker image ls` to confirm the image was successfully built.

- Now you need to push that image to a registry (`docker image push DOCKERUSER/REPO:TAG .`, ex. `docker image push nigelpoulton/gsd:swarm2023 .`)

- Once that image is pushed to a registry, you can now deploy the app with `docker stack deploy -c compose.yml counter`

- ### Deploy Explanation&colon;

  - `docker stack deploy` denotes a stack is going to be deployed.
  
  - `-c compose.yml` denotes it will be built with a compose file, and that file's name is **compose.yml**
  
  - `counter` is the name of the stack. It can be whatever you want, but it should describe what the stack does.

- You can then run `docker stack ls` to find a list of all created stacks.

- You can get more detail on each of the services using `docker stack services SERVICENAME`.

- You can see each replica using `docker stack ps counter`.

- You can access the app from any of the nodes by opening a web browser & in the address field, you enter the node's IP Address, followed by the port you find with `docker stack services SERVICENAME`. Example: `123.45.67.890:5001`.

- The site that opens shows the pod/container/host's name that serviced the request. If you refresh the page a few times, you will see that name change. This is called "**load-balancing**" where the load is shared across the different nodes so they all support each other.

- The recommended way to make any changes to declarative apps is to modify the declarative compose.yml config file & then resend those changes to the swarm. This ensures your config file(s) will stay consistent and up to date with the production environment. This works the same way if you are using Kubernetes instead of Docker Swarm.

- To modify it, enter a node's shell (`multipass shell node1`), then enter the **gsd/swarm** folder (`cd gsd/swarm`), then run `vi compose.yml` to enter the compose.yml file, then tap the **INSERT** key on your keyboard, then make your changes, then press the **ESC** key to exit the **INSERT** mode, then go to the bottom & type `:wq` to save & exit the file, then redeploy the service (`docker stack deploy -c compose.yml counter`).

- **For example:** If you modify the replicas from 10 down to 4 & then redeploy, Docker Swarm will remove the 6 unneeded containers. Then you can run `docker stack services counter` to verify the replicas were changed correctly.

- To see the self-healing again, you can simulate a node failure by running `multipass delete NODENAME`, then run `multipass list` to confirm the node is deleted, then enter a node's shell (`multipass shell node1`), then run `docker stack ps counter`, and you will see the containers that used to be running on that deleted node were now moved over to a still running node.

- This is the end of the Getting Started with Docker PluralSight Class by Nigel Poulton. To clean up, you need to exit the node's shell if you're still in it, then remove the stack (`docker stack rm counter`, then remove the cluster `multipass delete node1 node2 node3 node4`) (node5 was already deleted), then purge the cluster (`multipass purge`), then run `multipass list` to confirm the cluster has been removed (PowerShell should say "**No Instances Found**").

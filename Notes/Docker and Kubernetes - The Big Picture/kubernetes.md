![Kubernetes Logo](/images/kubernetes/kubernetes-logo.png)

# Kubernetes

This file contains notes for the "**Kubernetes**" section of the [Docker and Kubernetes: The Big Picture](https://app.pluralsight.com/library/courses/docker-kubernetes-big-picture/table-of-contents) PluralSight Class by Nigel Poulton.

## Summary

- Kubernetes is all about managing containerized apps at scale, and the focus is on the app.

- Kubernetes is completely open source & is the poster child for the Cloud Native Computing Foundation (CNCF, they lead the development & adoption of Cloud Native Technologies).

- Kubernetes is everywhere & almost all big tech companies use Kubernetes.

- Kubernetes can run anywhere, cloud, on-premises, laptop, etc.

- Kubernetes needs upfront work but after that (set packaging, threshold, etc.), it is almost completely independent.

- Kubernetes decouples apps from the environment which lets it switch between clouds/on-prem/laptop easily.

## ----------------------- Deeper Explanation -----------------------

## Kubernetes History

- Kubernetes is a very extensive platform & can do a lot of things, like stateless, stateful, batch work, long running, security, storage, networking, serverless, functions as a service, machine learning, & a lot more.

- Kubernetes was made by Google & has an illustrious heritage of other tools (1st Borg, 2nd Omega, 3rd Kubernetes) that also managed apps at massive scale.

- There is not a lot that Kubernetes can't do. The stuff it can do can be done anywhere (cloud, on-premises, data center, laptop, etc.)

- Kubernetes is Greek for "**helmsman**" or "**captain**", the one who steers the ship. It is often abbreviated to **k8s** (#8 replacing the 8 letters between K & S, pronounced "keights").

## Kubernetes: The Short and Skinny

- At its core, Docker provides the mechanics to start, stop, & manage individual containers which is low-level stuff.

- Kubernetes doesn't care about low-level stuff & instead does high-level stuff such as how many containers to run in, which nodes to run them on, when to scale up or down, and how to update containers with little/no downtime.

- To use an analogy: orchestras have a bunch of different relatively small sections (woodwind, brass, etc.) that all do their own thing while being directed by the conductor. With this analogy, applications are the orchestra, microservices are the sections, & Kubernetes is the conductor. It issues commands to Docker instances, telling them how/when to start & stop & commands to manage containers.

- Applications are run as individual nodes which runs a Kubernetes agent (like an orchestra section leader) and a container runtime (like individual players, usually Docker or Containerd but others exist too).

- Sitting above this cluster is the "**Kubernetes Control Pan**e" which makes descisions like the orchestra conductor.

    ![Kubernetes Control Pane & Cluster](/images/kubernetes/kubernetes-cluster-control-pane.png)

- Overall, Kubernetes is INSANELY smart and changes how everything runs, on the fly.

- If your apps are running with Kubernetes, it is seamless to move them on/off of a cloud or to/from other clouds.

- This does get hard if you write your services to be tightly coupled to the features of a specific cloud, but this is bad practice & should be avoided.

- Say your application has a **CPU/RAM heavy backend** and a **CPU/RAM light frontend**. You tell Kubernetes to allocate a one big node for the backend and two smaller nodes for the frontend. To start off, Kubernetes sets up exactly as many nodes as directed. Say one end's load increases and it's getting bogged down, Kubernetes is smart enough to auto-spin-up more nodes & spread the slow end's load across those new nodes which speeds up that end. If the load decreases, Kubernetes will auto-remove the unused nodes.

- Slow:

    ![Kubernetes Slow Nodes](/images/kubernetes/kubernetes-slow.png)

- Fast:

    ![Kubernetes Fast Nodes](/images/kubernetes/kubernetes-fast.png)

- If nodes fail & now you have less than the desired amount of nodes, Kubernetes will spin up as many nodes as needed to reach the desired amount, called self-healing.

    ![Kubernetes Failed & Healed Node](/images/kubernetes/kubernetes-failed-healed.png)

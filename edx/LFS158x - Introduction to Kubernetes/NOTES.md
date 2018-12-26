# Chapter 1: Generics
## Container Images
Bundle the application along with it's runtime and dependencies

## Container Orchestrators
### Definition
The tools which group hosts together to form a cluster.

### Reasoning
Can be managed by a set of scripts, but orchestrators give you:

- Bring multiple hosts together and make them part of a cluster
- Schedule containers to run on different hosts
- Help containers running on one host reach out to containers running on other hosts in the cluster
- Bind containers and storage
- Bind containers of similar type to a higher-level construct, like services, so we don't have to deal with individual containers
- Keep resource usage in-check, and optimize it when necessary
- Allow secure access to applications running inside containers.

### Implementations
- Docker Swarm
- Kubernetes
- Mesos Marathon
- Amazon ECS
- Hashicorp Nomad

# Chapter 2: Kubernetes (k8s)
## History
### Definition
> Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

### Etmology
Comes from Greek word for *helmsman* or *ship pilot* and can thus be considered a manager for shipping containers

### Origins
Inspired by Google's *Borg*, and as such is written in *Go*. From *Borg* it draws the following core features:

- API Servers
- Pods
- IP-per-Pod
- Services
- Labels

### Features
- **Automatic binpacking**

  *Kubernetes automatically schedules the containers based on resource usage and constraints, without sacrificing the availability.*

- **Self-healing**

  *Kubernetes automatically replaces and reschedules the containers from failed nodes. It also kills and restarts the containers which do not respond to health checks, based on existing rules/policy.*

- **Horizontal scaling**

  *Kubernetes can automatically scale applications based on resource usage like CPU and memory. In some cases, it also supports dynamic scaling based on customer metrics.*

- **Service discovery and Load balancing**

  *Kubernetes groups sets of containers and refers to them via a DNS name. This DNS name is also called a Kubernetes service. Kubernetes can discover these services automatically, and load-balance requests between containers of a given service.*
  
- **Automated rollouts and rollbacks**

  *Kubernetes can roll out and roll back new versions/configurations of an application, without introducing any downtime.*

- **Secrets and configuration management**

  *Kubernetes can manage secrets and configuration details for an application without re-building the respective images. With secrets, we can share confidential information to our application without exposing it to the stack configuration, like on GitHub.*

- **Storage orchestration**

  *With Kubernetes and its plugins, we can automatically mount local, external, and storage solutions to the containers in a seamless manner, based on Software Defined Storage (SDS).*

- **Batch execution**

  *Besides long running jobs, Kubernetes also supports batch execution.*
  
### License
Apache License Version 2.0

# Chapter 3: Kubernetes Architecture
## Master Node(s)
The **Master Node(s)** manage the Kubernetes cluster by acting as the gateway for all adminstrative tasks and can be accessed via CLI, GUI or API.

### Components
* **API Server**
  
  *All administrative tasks are submitted via the API Server via REST commands. The API Server validates and processes these requests, updating the state of the cluster in the distributed key-value store upon completion.*
  
* **Secheduler**

  *Work is scheduled to different Pods and Services based on resource usage and  user/operator prescribed constraints, data locality, affinity, etc.*
  
* **Controller Manager**

  *Several non-terminating control loops monitor and regulate the current state of the cluster via the API Server. Should any loop find that the desired state of the objects it manages do not meet the desired state, it takes corrective steps and then resumes normal operations.*
  
* **etcd**

  *Each cluster contains either a local or distributed key-value store that stores the state of the cluster.*
 
## Worker Node(s)
The **Worker Node(s)** are machines (VM, physical server, etc.) which run applications using Pods. A Pod is the core scheduling unit in Kubernetes which consists of a logical collection of one or more containers which are **always scheduled together**.

### Components
* **Container Runtime (CR)**

  *A Worker Node requires a Container Runtime (e.g. Docker, rkt, etc.) to schedule pods.*
  
* **kubelet**

  *The kubelet communicates between the Master Node and the Worker Node by:*

  1. Receiving Pod definitions and running the associated containers associated via the CR.
  2. Ensuring the containers with make up the Pod are healthy at all times.

* **kube-proxy**

  *The network proxy which runs on each node and listens to the API Server so it may create and delete Service endpoints as Pods come into and out of service.*

## Key-Value Store (etcd)
**etcd** is a distributed key-value store, written in Go, which uses the Raft Consensus algorithm to relibably stores the state (e.g. subnets, ConfigMaps, Secrets, etc.) of the Kubernetes cluster at any given time.

## Networking
### Unique IPs
Each Pod requires a unique IP address, which the CR offloads to Container Network Interface (CNI).

### Container-to-Container Communication
The CR creates a unique network identifier for each container that it starts which is referred to as a Network Namespace. These Network Namespaces can be shared across multiple containers, or with the Host Operating System. Within a Pod, Network Namespaces are shared so that containers may communicate via localhost.

# Chapter 4: Kubernetes Configuration
Kubernetes is often installed in one of four main confiugrations:

* **All-in-One Single-Node**

  *In this configuration the Master and Worker components are installed side by side, which is great for learning, but is rather brittle and should not be used in production. `minikube` is a great example of this type of installation*

* **Single-Node etcd, Single-Master, Multi-Worker**

  *In this configuration there is a single Master, which also runs a single node `etcd` instace, and multiple Workers with connect to the Master remotely.*
  
* **Single-Node etcd, Multi-Master, Multi-Worker**

  *In this configuration there are multiple Masters operating in a high availability mode with a shared single-node `etcd` instance and multiple Workers connected to the Masters remotely.*

* **Multi-Node etcd, Multi-Master, Multi-Worker** (recommended for **production**)
  
  *In this configuration there are multiple Masters operating in a high availability mode with multiple Workers connected to the Masters remotely. The main difference in this configuation is that `etcd` is configured in clustered mode outside of the Kubernetes cluster and is connected to remotely* 
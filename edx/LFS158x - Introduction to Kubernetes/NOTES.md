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

  _Kubernetes automatically schedules the containers based on resource usage and constraints, without sacrificing the availability._

- **Self-healing**

  _Kubernetes automatically replaces and reschedules the containers from failed nodes. It also kills and restarts the containers which do not respond to health checks, based on existing rules/policy._

- **Horizontal scaling**

  _Kubernetes can automatically scale applications based on resource usage like CPU and memory. In some cases, it also supports dynamic scaling based on customer metrics._

- **Service discovery and Load balancing**

  _Kubernetes groups sets of containers and refers to them via a DNS name. This DNS name is also called a Kubernetes service. Kubernetes can discover these services automatically, and load-balance requests between containers of a given service._
  
- **Automated rollouts and rollbacks**

  _Kubernetes can roll out and roll back new versions/configurations of an application, without introducing any downtime._

- **Secrets and configuration management**

  _Kubernetes can manage secrets and configuration details for an application without re-building the respective images. With secrets, we can share confidential information to our application without exposing it to the stack configuration, like on GitHub._

- **Storage orchestration**

  _With Kubernetes and its plugins, we can automatically mount local, external, and storage solutions to the containers in a seamless manner, based on Software Defined Storage (SDS)._

- **Batch execution**

  _Besides long running jobs, Kubernetes also supports batch execution._
  
### License
Apache License Version 2.0

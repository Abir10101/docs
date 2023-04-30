# Kubernates

## Questions
- What is Kubernates?
- What are the components of Kubernates?
- What is Deployment?
- What is basic working of deployments?
- What are difference between Deployment vs Statefulset?
- What are difference between Replicaset vs Deployment?
- What are differnet types of probes and their usecases?
- What is Taints and Tolerant?
- What are the usecases of Labels and Selectors?
- What is initContainers?
- What are the types of services in k8s?
- How persisted volumes work?
- How to dynamically scale k8s cluster?
- How to install k8s to a node and add it the pool?
---
<br>
<br>

## What is Kubernates?
Kubernates is a orchestartion tool for containers. <ins> Orchestration </ins>: Spining up multiple containers in a ordered way for production level deployment i.e. horizontal and vertical scaling, Specific machines for specific workloads, Respawning unhealthy containers, etc. Containers include: Logging containers, Mailing Containers, etc.
<br>
<br>

## What are Components of Kubernates?
Some basic components of kubernates include:
- **Pods**: A abstraction layer above containers. Pods contains container runtime for spining up containers.
- **Nodes**: These a basically physical machines or virtual machines where the kubernates is installed and added to the pool for spinning up pods.
- **Deployments**: These are manifests for the stateless application written in yaml file. This is used for packaging the application, scaling application, creating replicas containers, health check containers.
- **StatefulSet**: These are manifests for stateful applications such as databases. We generally don't use statefulsets for deploying databases as we have to create master slave architecture for fetching and updating data.
- **ConfigMap**: 
- **Secrets**: 
- **Services**: 
- **NameSpace**: 
- **StorageClass**: 
- **PersistedVolumes**: 

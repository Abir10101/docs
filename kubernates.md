# Kubernates

### Questions
- What is Kubernates?
- What are the components of Kubernates?
- What is Deployment?
- What are difference between Deployment vs Statefulset?
- What are difference between Replicaset vs Deployment?
- What are differnet types of probes and their usecases?
- What is Taints and Tolerant?
- What are the usecases of Labels and Selectors?
- What is initContainers?
- What are the types of services in k8s?
- What is StorageClass?
- What are the components of StorageClass?
- How persisted volumes work?
- How to dynamically scale k8s cluster?
- How to install k8s to a node and add it the pool?
---
<br>
<br>

## What is Kubernates?
Kubernates is a orchestartion tool for containers. <ins> Orchestration </ins>: Spining up multiple containers in a ordered way for production level deployment, i.e., horizontal and vertical scaling, Specific machines for specific workloads, Respawning unhealthy containers, etc. Containers include: Logging containers, Mailing Containers, etc.
<br>

## What are Components of Kubernates?
Some basic components of kubernates include:
- **Pods**: A abstraction layer above containers. Pods contains container runtime for spining up containers.
- **Nodes**: These are physical or virtual machines in which the kubernates is installed and added to the pool for spinning up pods.
- **Deployments**: These are manifests for the stateless application written in yaml file. This is used for packaging the application, scaling application, creating replicas containers, health check containers.
- **StatefulSet**: These are manifests for stateful applications such as databases. We generally don't use statefulsets for deploying databases as we have to create master slave architecture for fetching and updating data.
- **ConfigMap**: These are manifests for applying config variables that can be used in other manifests file. Example: database_host, database_port, etc. to be used in deployment manifests.
- **Secrets**: These are manifests for applying variables that are encoded (base64). For example: database_password, database_username for using in database pods manifests.
- **Services**: These are gateways to enable network connection in and out of the pods. These select pods to attach to by using selectors of the pods.
- **NameSpace**: These are used to isolate group of resources of application within a cluster. For example: Application pods is deployed in a namespace(ns1) and monitering tools for the pods is deployed in another namespace(ns2).
- **StorageClass**: These defines the config of the storage required. For example: type of storage, provisoner of the storage, etc.
- **PersistedVolumes**: These are the storage outside of the cluster which is going to persist after pods deletion. These are used to persist data in database. These work by attaching with StorageClass.
<br>

## What is Deployment?
- Deployment is a higher-level abstraction that manages the lifecycle of a ReplicaSet.

- A Deployment is responsible for creating and updating ReplicaSets, which in turn create and manage the Pods that run the application.

- Deployment YAML file includes information such as the container image to use, the number of replicas to run, and any environment variables or volumes needed by the application.

- Deployments also provide features for scaling the application up or down, rolling back to a previous version, and pausing or resuming updates.
<br>

## What are difference between Deployment vs Statefulset?
- Deployments manage stateless applications whereas Statefulset are designed to manage stateful applications.

- In Deployment, Pods are interchangeable and has random unique identifier.  
In StatefulSet, Pods are not interchangeable. Each pod has a persistent and sequential ordered identifier.

- When we update a Deployment, it will create a new ReplicaSet. Then, it'll gradually move the Pods from the old ReplicaSet. If an error occurs while updating, the new ReplicaSet will never be in Ready state. The old ReplicaSet will not terminate, ensuring 100% uptime. We can also manually rollback to a previous ReplicaSet in case our new feature is not working as expected.  
StatefulSets don’t create ReplicaSet, so we cannot rollback. StatefulSet aslo performs RollingUpdate, in a manner that, one replica pod will go down first, and then only updated pod will spin up.

- Deployment uses PersistentVolumeClaim which is shared by all pod replicas (shared volume).  
Statefulset used volumeClaimTemplates so that each replica pod gets a unique PVC and PV associated with it. In other words, no shared volume.
<br>

## What are difference between Replicaset vs Deployment?
- ReplicaSet is a lower-level object than Deployment. It provides a basic level of functionality for managing of replicas/pods.  
Deployment is a higher level of abstraction that uses ReplicaSet to manage and maintain a set of replicas/pods. It has more advanced features like rolling update and rollback of pods, and is recommended for most applications.


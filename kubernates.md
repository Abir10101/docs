# Kubernates

### Questions
- What is Kubernates?
- What are the components of Kubernates?
- What is Deployment?
- What are difference between Deployment vs Statefulset?
- What are difference between Replicaset vs Deployment?
- What are differnet types of probes and their usecases?
- What are initContainers?
- What is Taints and Tolerant?
- What are Labels and Selectors?
- What are the types of services in k8s?
- What is a Headless service?
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
<br>

## What is Deployment?
- Deployment is a higher-level abstraction that manages the lifecycle of a ReplicaSet.

- A Deployment is responsible for creating and updating ReplicaSets, which in turn create and manage the Pods that run the application.

- Deployment YAML file includes information such as the container image to use, the number of replicas to run, and any environment variables or volumes needed by the application.

- Deployments also provide features for scaling the application up or down, rolling back to a previous version, and pausing or resuming updates.
<br>
<br>

## What are difference between Deployment vs Statefulset?
- Deployments manage stateless applications whereas Statefulset are designed to manage stateful applications.

- In Deployment, Pods are interchangeable and has random unique identifier.  
In StatefulSet, Pods are not interchangeable. Each pod has a persistent and sequential ordered identifier.

- When we update a Deployment, it will create a new ReplicaSet. Then, it'll gradually move the Pods from the old ReplicaSet. If an error occurs while updating, the new ReplicaSet will never be in Ready state. The old ReplicaSet will not terminate, ensuring 100% uptime. We can also manually rollback to a previous ReplicaSet in case our new feature is not working as expected.  
StatefulSets don’t create ReplicaSet, so we cannot rollback. StatefulSet aslo performs RollingUpdate, in a manner that, one replica pod will go down first, and then only updated pod will spin up.

- Deployment uses PersistentVolumeClaim which is shared by all pod replicas (shared volume).  
Statefulset uses volumeClaimTemplates so that each replica pod gets a unique PVC and PV associated with it. In other words, no shared volume.
<br>
<br>

## What are difference between Replicaset vs Deployment?
- ReplicaSet is a lower-level object than Deployment. It provides a basic level of functionality for managing of replicas/pods.  
Deployment is a higher level of abstraction that uses ReplicaSet to manage and maintain a set of replicas/pods. It has more advanced features like rolling update and rollback of pods, and is recommended for most applications.
<br>
<br>

## What are differnet types of probes and their usecases?

### Liveness Probe:
This probe basically answers the question 'Is the container services is running properly?'. If not then restart the container. It will poll for the lifetime of a pod. Example:
```
livenessProbe:
    httpGet:
        path: /health
        port: 8080
    periodSeconds: 30
    timeoutSeconds: 5
```
The probe would make an HTTP request to a specific endpoint(/health) on the web server every 30 sec. If the web server doesn't respond within 5 sec, k8s will restart the container.

### Readiness Probe:
This probe basically answers the question 'Is the pod ready to serve traffic?'. If not then the pod will be removed from load balancing network. It will poll for the lifetime of a pod. Example:
```
readinessProbe:
    tcpSocket:
        port: 3306
    periodSeconds: 10

```
check if the database server is ready to accept traffic. The probe could make a TCP connection to the database server every 10 seconds. If the connection fails, k8s will not serve any traffic to the pod and will not add it to the service's load balancer.

### Startup Probe:
Startup Probe: It basically answers the question 'Is the pod is ready?'. If not, in a defined time, then K8s will restart the pod. All other probes are disabled until the startup probe succeds. This is useful if the container takes a long time to start up. It ran once when the pod is spinning up, once succeds k8s will move to other probes. Example:
```
startupProbe:
    httpGet:
        path: /init
        port: 8080
    failureThreshold: 30
    periodSeconds: 10
```
The probe could make an HTTP request to a specific endpoint(/init) every 10 seconds wait period for 30 times before failing. If it fails, k8s will restart the pod.
<br>
<br>

## What are initContainers?
- It is a type of container that runs and completes its work before the main application containers start running.
- They are typically used for pre-initialization tasks such as setting up configuration for the main application.
- It run in the same pod as the main application container, which allows them to share the same volumes and network namespace. Example:
```
initContainers:
- name: init-db
    image: busybox
    imagePullPolicy: IfNotPresent
    env:
    - name: DB_URL
        valueFrom:
        configMapKeyRef:
            name: mysql-configmap
            key: db_url
    command: ['sh', '-c', "until nslookup $(DB_URL).$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace).svc.cluster.local; do echo waiting for database; sleep 2; done"]
```
This checks for the database endpoint to be ready before initializing the main application.
<br>
<br>

## What is Taints and Tolerant?
These are used to control the placement of pods on nodes.

- Taints: It is a property of a Node that repels pods. It indicates that the node has certain limitations or requirements, such as need for specialized hardware.
- Tolerations: It is a property of a Pod that allows it to be scheduled on a node with a matching taint.

Example:  
Let a k8s cluster with Nodes: node1, node2, and node3. Now mark node1 as "special" and only want to run certain pods on that node.
```
kubectl taint nodes node1 key=value:NoSchedule
```
Now let a pod to run on node1. Add a toleration to that pod as follows:
```
tolerations:
- key: "key"
  operator: "Equal"
  value: "value"
  effect: "NoSchedule"
```
<br>
<br>

## What are Labels and Selectors?
Labels and Selectors are used to identify and select a set of objects. These are used to organize and manage k8s objects.

- Labels are key-value pairs that are attached to k8s objects, used to identify and group objects that have something in common.  
Example: Label `app=frontend` to all the pods that belong to the frontend application.
- Selectors are used to identify a group of objects based on their labels. A Selector is a set of rules that defines a set objects to be selected.  
Example: Selector `app=frontend` to select all the pods that belong to the frontend application.
<br>
<br>

## What are the types of services in k8s?
Services that can be used to expose applications running within a cluster. The commonly used types of services:

- ClusterIP: This is the default service type. It exposes the service on a cluster-internal IP address. It allows communication between different services within the cluster but does not expose the service externally.  
Example:
```
apiVersion: v1
kind: Service
metadata:
  name: service_name
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

- NodePort: This service exposes a static port on every node in the cluster. It creates a mapping between a port on the node's IP address and the port of the service. This allows external access to the service using the node's IP address and the static port.  
Example:
```
apiVersion: v1
kind: Service
metadata:
  name: service_name
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30000
```
- LoadBalancer: This service provisions an external load balancer, such as a cloud provider's load balancer, to distribute traffic across the available pods, using various load balancing algorithms. If a pod becomes unhealthy or is removed from the cluster, the load balancer stops sending traffic to that pod until it becomes healthy again or a new pod is created. It automatically assigns an external IP address to the service, allowing it to be accessible from outside the cluster.  
Example:
```
apiVersion: v1
kind: Service
metadata:
  name: service_name
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
- ExternalName: This service is used to reference an external Domain outside of the cluster. It acts as a DNS alias (DNS CNAME record), for the external service. Instead of hard-coding IP addresses or DNS names directly in application code, you can use an ExternalName service as an abstraction layer to reference to the domain name. This enables you to switch or update the external resource without modifying your application code.  
Example:
```
apiVersion: v1
kind: Service
metadata:
  name: service_name
spec:
  type: ExternalName
  externalName: example.com
```
<br>
<br>

## What is a Headless service?
- Unlike regular services, Headless services do not have a Cluster IP assigned to them. This means that the service is not load balanced or reachable through a single IP address.
- With a Headless service, each Pod backing the service is assigned a DNS record in the form of `<pod-name>.<service-name>.<namespace>.svc.cluster.local`. This allows other Pods within the same cluster to directly address individual Pods by their DNS names.
- Use Cases: Headless services are commonly used in scenarios where direct communication with individual Pods is required. This is useful in StatefulSets, where each Pod may have its own unique identity or data that needs to be accessed directly.  
Example:
```
apiVersion: v1
kind: Service
metadata:
  name: service_name
spec:
  clusterIP: None # <--
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```







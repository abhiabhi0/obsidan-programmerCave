## What is the role of a Kubernetes control plane, and can you explain its main components and their responsibilities?

The **Kubernetes control plane** is the central management entity responsible for maintaining the desired state of a Kubernetes cluster. It makes global decisions about the cluster, such as scheduling pods, and detects/responds to cluster events (e.g., a pod crashing). Let’s break down its primary components:
### 1. **kube-apiserver**
- **Role**: Acts as the front end for the Kubernetes control plane.
- **Responsibility**:
    - Handles REST API requests from kubectl, other internal components, or external services.
    - Validates and processes requests, updating the cluster state via etcd.
### 2. **etcd**
- **Role**: A key-value store used as Kubernetes' backing store.
- **Responsibility**:
    - Stores all cluster data, including configuration details, the state of nodes, pods, secrets, and config maps.
    - Ensures consistent data storage across the cluster.
### 3. **kube-scheduler**
- **Role**: Assigns pods to nodes.
- **Responsibility**:
    - Watches for unscheduled pods and selects appropriate nodes based on constraints (like resource availability and affinity/anti-affinity rules).
### 4. **kube-controller-manager**
- **Role**: Runs controller processes.
- **Responsibility**:
    - Ensures the cluster is in its desired state.
    - Manages different controllers, such as the Node Controller (for detecting node failures), Replication Controller (for managing pod replicas), and others.
### 5. **cloud-controller-manager** (optional in some environments)
- **Role**: Integrates with cloud-specific APIs.
- **Responsibility**:
    - Manages cloud-provider-specific resources, such as load balancers or storage volumes.
[[0100 - Architecture]]
---
## In Kubernetes, what is a DaemonSet, and when would you use it?
[[0301 - DaemonSets in Kubernetes]]

---
## How does Kubernetes handle service discovery and load balancing within a cluster? Explain the concepts of ClusterIP, NodePort, and LoadBalancer services.
[[0203 - Service Discovery in Kubernetes]]
[[0204 - Load Balancing in Kubernetes]]
[[0302 - Kubernetes Service Types - ClusterIP, NodePort, and LoadBalancer]]

Kubernetes provides built-in **service discovery** and **load balancing** mechanisms to enable communication between applications running in a cluster. Services in Kubernetes expose a set of pods as a network service, providing a stable endpoint for clients despite changes in the underlying pod IPs.
### 1. **ClusterIP (Default Service Type)**
**Description**:
- Exposes the service on a virtual IP (internal to the cluster).
- Accessible only from within the cluster.
**Use Case**:
- Ideal for communication between microservices within a cluster.
- Example: A backend service that is consumed by a frontend service internally.
### 2. **NodePort**
**Description**:
- Exposes the service on a static port on each node’s IP address.
- The service becomes accessible externally using `NodeIP:NodePort`.
**Use Case**:
- Suitable for development or scenarios where a basic external access mechanism is needed.
- Example: Running a simple demo service that doesn’t require complex load balancing.
### 3. **LoadBalancer**
**Description**:
- Uses a cloud provider’s load balancer to expose the service externally.
- Provides a public IP and automatically configures the load balancer.
**Use Case**:
- Best for production environments where scalable external traffic management is required.
- Example: Exposing a web application or an API service to users outside the cluster.
### Summary of Differences

| Service Type | Accessibility               | Typical Use Case                           |
| ------------ | --------------------------- | ------------------------------------------ |
| ClusterIP    | Internal to cluster         | Communication between internal services    |
| NodePort     | External via node IP & port | Direct external access, small-scale apps   |
| LoadBalancer | External, cloud-managed     | Publicly accessible services in production |

---
## What is a Kubernetes StatefulSet, and how does it differ from a Deployment?
A **StatefulSet** manages the deployment and scaling of a set of pods, providing **guaranteed ordering and uniqueness** for each pod. Unlike a Deployment, which treats all pods as interchangeable, StatefulSet maintains a unique identity for each pod, which is important for stateful applications.
### Key Differences Between StatefulSet and Deployment

|Feature|**StatefulSet**|**Deployment**|
|---|---|---|
|Pod Identity|Each pod has a stable, unique identifier (name and storage).|All pods are identical and interchangeable.|
|Persistent Storage|Supports **persistent volume claims (PVCs)** per pod. Each PVC is uniquely associated with a pod.|PVCs are shared across all pods or dynamically allocated.|
|Pod Ordering|Ensures sequential startup, scaling, and termination.|No guarantee of pod startup or termination order.|
|Use Case|Stateful applications (databases, message queues).|Stateless applications (web servers, APIs).|
### Example Use Case for StatefulSet
Imagine you are deploying a **MySQL** cluster where:
- Each database node needs a **persistent volume** to store data across restarts.
- The nodes require a **stable network identity** so they can communicate with one another correctly.
- Pods must start in a specific order to establish master-slave replication.

In this case, using a StatefulSet ensures that:
- Each MySQL pod (`mysql-0`, `mysql-1`, `mysql-2`) has its own persistent volume claim.
- The pod names remain consistent even if pods are deleted or rescheduled.
- Pods start in the correct order to maintain the integrity of the cluster.
**Example StatefulSet for MySQL**:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  serviceName: "mysql"
  replicas: 3
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```
### When to Use StatefulSet Over Deployment
- **Databases**: MySQL, MongoDB, Cassandra.
- **Message Brokers**: Kafka, RabbitMQ.
- **Applications requiring stable storage and consistent naming**.
---
## How does Kubernetes implement resource limits and requests, and why are they important for pod scheduling and cluster stability?
In Kubernetes, **resource requests** and **limits** are used to manage **CPU** and **memory** resources for containers within pods. They help Kubernetes make intelligent decisions about pod scheduling and prevent overuse or starvation of resources, ensuring the stability of the cluster.
### **Resource Requests**
- **Definition**: The **minimum** amount of CPU or memory that a container needs to run.
- **Role in Scheduling**: Kubernetes uses resource requests to determine where to schedule the pod. When a pod is scheduled on a node, the node must have at least the requested amount of resources available for the pod to run.
- **Impact on Cluster**: Resource requests help prevent pods from being scheduled on nodes that don’t have enough available resources.
### **Resource Limits**
- **Definition**: The **maximum** amount of CPU or memory that a container can use.
- **Role in Runtime**: If a container tries to use more resources than its limit, Kubernetes will enforce these limits. For memory, if the container exceeds its memory limit, it may be terminated and restarted (OOMKilled). For CPU, if the container exceeds its CPU limit, it will be throttled, but not killed.
### **Why Are Resource Requests and Limits Important?**
1. **Efficient Scheduling**: Kubernetes uses requests to decide on the optimal node for a pod, ensuring it is scheduled where resources are available.
2. **Preventing Resource Starvation**: By setting resource requests and limits, you can prevent a single container from monopolizing resources, which could affect other containers running on the same node.
3. **Cluster Stability**: Limits help ensure that a misbehaving or poorly configured container doesn’t consume too much of the node’s resources, leading to crashes or performance degradation for other services.
4. **Quality of Service (QoS) Class**:
    - **Guaranteed**: A pod has both memory and CPU requests and limits set to the same value.
    - **Burstable**: A pod has a resource request and a limit, but they’re different.
    - **BestEffort**: A pod has no resource requests or limits set.
---
## What is Kubernetes Horizontal Pod Autoscaler (HPA), and how does it work?
The **Horizontal Pod Autoscaler** is a Kubernetes resource that automatically adjusts the number of pod replicas in a deployment or replica set based on observed metrics, such as CPU usage or memory utilization. This allows applications to scale dynamically in response to traffic or load changes.
### How HPA Works:
1. **Metrics Collection**: The HPA continuously monitors the resource usage of the pods it controls (e.g., CPU, memory).
2. **Scaling Decision**: When the observed metric exceeds the predefined threshold (e.g., average CPU usage > 80%), the HPA will scale up the number of pods. Conversely, if the resource usage falls below the threshold, the HPA will scale down the pods.
3. **Autoscaling Mechanism**: The HPA calculates the desired number of pods by using the formula:
    - Desired replicas = Current replicas × (Current utilization / Desired utilization)
### Metrics for Scaling:
The most common metrics used for scaling are:
1. **CPU utilization** (measured as a percentage of the requested CPU).
2. **Memory utilization** (measured as a percentage of the requested memory).
3. **Custom Metrics** (using tools like the **Metrics Server** or **Prometheus Adapter**) for application-specific metrics (e.g., request count, latency).

The HPA adjusts the number of pods based on the defined metric thresholds, helping ensure that applications have the resources they need under varying load conditions.
### Benefits of Using HPA:
- **Efficient Resource Utilization**: Ensures resources are efficiently used by automatically scaling applications based on real-time load.
- **Cost Efficiency**: Helps to save resources and reduce costs by scaling down unused resources during low traffic periods.
- **Improved User Experience**: By scaling up resources during high traffic, the system ensures that applications remain responsive even under heavy load.
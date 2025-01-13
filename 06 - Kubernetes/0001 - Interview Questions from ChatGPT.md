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

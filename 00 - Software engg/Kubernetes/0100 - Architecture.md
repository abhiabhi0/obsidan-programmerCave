## Architecture of Kubernetes

Kubernetes is a powerful container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. Its architecture consists of a **Control Plane** and **Worker Nodes**, each with specific components that work together to maintain the health and performance of the cluster.

![[Kubernetes-Architecture.png]]

### 1. Control Plane Components

The Control Plane is responsible for managing the overall state of the Kubernetes cluster. It includes several key components:

- **kube-apiserver**: This is the central component that exposes the Kubernetes API. It processes and validates API requests, serving as the primary interface for users and other components to interact with the cluster[1][3].

- **etcd**: A distributed key-value store that holds all cluster data, including configuration data and the state of various objects in Kubernetes. It ensures consistency and high availability[1][5].
- [[0104 - etcd in Kubernetes|etcd]]

- **kube-scheduler**: This component watches for newly created Pods that have no assigned node and selects a suitable node for them based on resource availability and other constraints[1][4].

- **kube-controller-manager**: It runs controllers that regulate the state of the cluster, ensuring that the desired state matches the actual state by performing tasks such as scaling Pods or managing node health[1][3].

- **cloud-controller-manager (optional)**: Integrates with cloud service providers to manage resources such as load balancers or storage volumes specific to cloud environments[1].

### 2. Worker Node Components

Worker Nodes are the machines where application workloads run, encapsulated in Pods. Each Worker Node contains several essential components:

- **kubelet**: An agent that communicates with the Control Plane and ensures that containers are running in a Pod as intended. It manages Pod lifecycle events, including starting, stopping, and monitoring containers[5][8].

- **kube-proxy**: A network proxy that maintains network rules for Pods, enabling communication between them. It handles service abstraction by directing traffic to appropriate Pods based on their IP addresses and ports[4][5].

- **Container Runtime**: This is responsible for running containers on a Node. Common container runtimes include Docker, containerd, and CRI-O. The runtime pulls container images from repositories and manages their execution[7][8].

[[0102 - What is a Pod in Kubernetes|Pods]]
### 3. Nodes in Kubernetes

A Kubernetes cluster consists of at least one Master Node (Control Plane) and one or more Worker Nodes:

- **Master Node**: Controls the cluster's overall operations, making global decisions about scheduling and managing resources.

- **Worker Node**: Executes application workloads packaged as Pods. Each Worker Node can run multiple Pods containing one or more containers[2][6].

[[0103 - Relationship Between Container, Pod, Node, Cluster, and Other Components in Kubernetes|Relationship Between Container, Pod, Node, Cluster, and Other Components in Kubernetes]]
### 4. Communication Between Components

The architecture follows a client-server model where:

- The **Control Plane** communicates with Worker Nodes using the kube-apiserver.
- The **kubelet** on each Worker Node regularly reports back to the Control Plane about the state of its Pods.
- The **kube-proxy** facilitates internal communication between different Pods across nodes.

### Summary

Kubernetes architecture is designed to provide a robust platform for managing containerized applications at scale. Understanding its components—both in the Control Plane and Worker Nodes—is crucial for effectively deploying and managing applications in a Kubernetes environment.

Citations:
[1] https://kubernetes.io/docs/concepts/overview/components/?origin_team=T08E6NNJJ
[2] https://www.simplilearn.com/tutorials/kubernetes-tutorial/kubernetes-interview-questions
[3] https://www.geeksforgeeks.org/kubernetes-architecture/
[4] https://k21academy.com/docker-kubernetes/most-asked-docker-kubernetes-interview-questions-and-answers/
[5] https://spacelift.io/blog/kubernetes-architecture
[6] https://www.geeksforgeeks.org/kubernetes-interview-questions/
[7] https://spot.io/resources/kubernetes-architecture/11-core-components-explained/
[8] https://www.turing.com/interview-questions/kubernetes
[9] https://k21academy.com/docker-kubernetes/kubernetes-architecture-components-overview-for-beginners/
[10] https://www.practical-devsecops.com/kubernetes-interview-questions/
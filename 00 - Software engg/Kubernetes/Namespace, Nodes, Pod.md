## Namespace

A [Kubernetes](https://www.aquasec.com/cloud-native-academy/kubernetes-101/kubernetes-complete-guide/) namespace helps separate a cluster into logical units. It helps granularly organize, allocate, manage, and secure cluster resources.

**Default Kubernetes namespaces**

Here are the four default namespaces Kubernetes creates automatically:

- **default**—a default space for objects that do not have a specified namespace.
- **kube-system**—a default space for Kubernetes system objects, such as kube-dns and kube-proxy, and add-ons providing cluster-level features, such as web UI dashboards, ingresses, and cluster-level logging.
- **kube-public**—a default space for resources available to all users without authentication.
- **kube-node-lease**—a default space for objects related to cluster scaling.

### When Should You Use Multiple Kubernetes Namespaces?

In small projects or teams, where there is no need to isolate workloads or users from each other, it can be reasonable to use the default Kubernetes namespace. Consider using multiple namespaces for the following reasons:

- **Isolation**—if you have a large team, or several teams working on the same cluster, you can use namespaces to create separation between projects and microservices. Activity in one namespace never affects the other namespaces.
- **Development stages**—if you use the same Kubernetes cluster for multiple stages of the development lifecycle, it is a good idea to separate development, testing, and production environments. You do not want errors or instability in testing environments to affect production users. Ideally, you should use a separate cluster for each environment, but if this is not possible, namespaces can create this separation.
- **Permissions**—it might be necessary to define separate permissions for different resources in your cluster. You can define separate RBAC rules for each namespace, ensuring that only authorized roles can access the resources in the namespace. This is especially important for mission critical applications, and to protect sensitive data in production deployments.
- **Resource control**—you can define resource limits at the namespace level, ensuring each namespace has access to a certain amount of CPU and memory resources. This enables separating cluster resources among several projects and ensuring each project has the resources it needs, leaving sufficient resources for other projects.
- **Performance**—Kubernetes API provides better performance when you define namespaces in the cluster. This is because the API has less items to search through when you perform specific operations.

## What Are Kubernetes Nodes?

A [Kubernetes](https://www.aquasec.com/cloud-native-academy/kubernetes-101/kubernetes-complete-guide/) node is a worker machine that runs [Kubernetes workloads](https://www.aquasec.com/cloud-native-academy/kubernetes-101/kubernetes-workloads/). It can be a physical (bare metal) machine or a virtual machine (VM). Each node can host one or more pods. Kubernetes nodes are managed by a control plane, which automatically handles the deployment and scheduling of pods across nodes in a Kubernetes cluster. When scheduling pods, the control plane assesses the resources available on each node.

Each node runs two main components—a kubelet and a container runtime. The kubelet is in charge of facilitating communication between the control plane and the node. The container runtime is in charge of pulling the relevant container image from a registry, unpacking containers, running them on the node, and communicating with the operating system kernel.

### Kubernetes Node Components
Here are three main Kubernetes node components:
#### kubelet 

The Kubelet is responsible for managing the deployment of pods to Kubernetes nodes. It receives commands from the API server and instructs the container runtime to start or stop containers as needed.

#### kube-proxy 

A network proxy running on each Kubernetes node. It is responsible for maintaining network rules on each node. Network rules enable network communication between nodes and pods. Kube-proxy can directly forward traffic or use the operating system packet filter layer. 

#### Container runtime 

The software layer responsible for running containers. There are several container runtimes supported by Kubernetes, including Containerd, CRI-O, Docker, and other Kubernetes Container Runtime Interface (CRI) implementations.

## Pods
A Kubernetes pod is a collection of one or more [Linux® containers](https://www.redhat.com/en/topics/containers), and is the smallest unit of a Kubernetes application. Any given pod can be composed of multiple, tightly coupled containers (an advanced use case) or just a single container (a more common use case). Containers are grouped into Kubernetes pods in order to increase the intelligence of resource sharing, as described below.
Kubernetes is an open-source container orchestration platform originally developed by Google and now maintained by the Cloud Native Computing Foundation (CNCF). It <mark class="hltr-g">automates the deployment, scaling, and management of containerized applications</mark>. Here are some key aspects of Kubernetes:

1. **[[Container Orchestration]]**:
   - Kubernetes allows you to manage and coordinate the deployment of containerized applications across a cluster of machines, abstracting away the underlying infrastructure.

2. **Declarative Configuration**:
   - Kubernetes uses YAML or JSON configuration files to describe the desired state of your application. You specify what you want to deploy, and Kubernetes takes care of making it happen.

3. **Scaling and Self-healing**:
   - Kubernetes can automatically scale your application based on resource usage or user-defined metrics using Horizontal Pod Autoscalers (HPA). It can also restart containers that fail, replace containers, kill containers that don't respond to your user-defined health check, and more, ensuring high availability and resilience.
   - Ref [[Horizontal and Vertical Scaling in Kubernetes]]

4. **Service Discovery and Load Balancing**:
   - Kubernetes provides built-in mechanisms for service discovery and load balancing. Services abstract away individual pod instances and provide a stable endpoint for accessing your application.

5. **Storage Orchestration**:
   - Kubernetes offers various options for persistent storage, including local storage, network-attached storage (NAS), and cloud-specific storage solutions. It enables you to dynamically provision storage resources and attach them to your pods.

6. **Secrets and Configuration Management**:
   - Kubernetes provides a secure way to store sensitive information such as passwords, OAuth tokens, and SSH keys using Secrets. It also supports ConfigMaps for storing non-sensitive configuration data.
   - Ref [[Secrets in Kubernetes]]

7. **Multi-Cloud and Hybrid Cloud Support**:
   - Kubernetes is cloud-agnostic and can run on any public cloud (like AWS, Google Cloud Platform, or Azure), on-premises, or even on bare-metal servers. This flexibility allows you to build applications that can easily be migrated between different environments.

8. **Extensibility and Ecosystem**:
   - Kubernetes has a rich ecosystem of third-party tools, plugins, and extensions that extend its functionality. These include monitoring and logging solutions, networking plugins (CNI), service meshes (like Istio), and more.

![[{8EF46AC2-4003-47F7-95E7-1228DC21FF3D}.png]]

A k8s cluster consists of a set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the Pods that are the components of the application workload. The control plane manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers, and a cluster usually runs multiple nodes, providing fault tolerance and high availability.

● Control Plane Components
1. API Server The API server talks to all the components in the k8s cluster. All the operations on pods are executed by talking to the API server.
2. Scheduler The scheduler watches pod workloads and assigns loads on newly created pods.
3. Controller Manager The controller manager runs the controllers, including Node Controller, Job Controller, EndpointSlice Controller, and ServiceAccount Controller.
4. etcd etcd is a key-value store used as Kubernetes' backing store for all cluster data.

● Nodes
1. Pods A pod is a group of containers and is the smallest unit that k8s administers. Pods have a single IP address applied to every container within the pod.
2. Kubelet An agent that runs on each node in the cluster. It ensures containers are running in a Pod.
3. Kube Proxy kube-proxy is a network proxy that runs on each node in your cluster. It routes traffic coming into a node from the service. It forwards requests for work to the correct containers.
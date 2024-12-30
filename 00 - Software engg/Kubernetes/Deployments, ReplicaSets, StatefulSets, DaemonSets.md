### Deployments

A Deployment is the preferred way to deploy an application inside a pod. It is a higher-level abstraction built on top of ReplicaSets that uses ReplicaSets internally to manage applications. In addition to the work carried out by a ReplicaSet, it provides added functionality such as:

- **Rolling Updates**: Rolling updates ensure that an application is updated gradually, one replica at a time, while ensuring that the overall availability of the application is not impacted. In comparison, ReplicaSets only support scaling and managing replicas.
- **Rollback**: Deployments automatically rollback to a previous version of an application if an update fails. For ReplicaSets, this process would need to be manually performed.
- **Version Control**: Similar to the previous feature, Deployments implement version control, hence allowing for the ability to rollback to a previous specific version.

Hence for such reasons, Deployments are the preferred way to go as they take care of a lot of the update and rollback functionality without any downtime and ensure that your application stays available and up to date. If you’d like to instead set up custom update functionality, then you could work with ReplicaSets.

### Sample Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

### ReplicaSets


The simplest unit in Kubernetes is the pod. We run our containers inside the pod. Say you’ve deployed your app inside a pod and you’re now getting huge traffic. So much that your single pod instance can’t handle it. How do you take care of that? Enter ReplicaSet. A ReplicaSet helps manage traffic by scaling your application to have multiple instances of the same pod. This helps reduce traffic to one particular instance and also helps in load-balancing traffic between each of these instances.

### Sample Manifest

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

- `.spec.replicas` denotes the number of replicas – the number of instances of the pod you would be running. This number would depend on how much you’d want to scale the pod.
- `.spec.selector` contains the `matchLabels` field among others which contains a map of key-value pairs used to match labels on pods. The ReplicaSet will then identify and manage any pods whose labels match the ones specified under this field.
- `.spec.template` contains the pod template that is used to create the replicas. These replicas are then managed by the ReplicaSet.
    - `.spec.template.metadata` contains labels which need to match those under the `.spec.selector.matchLabels` field for the ReplicaSet to identify said pod.
    - `.spec.template.spec` contains information for creating the desired pod. It has the container definition, and configuration options such as the volume needed, security context and more.

In a production-level setting, we wouldn’t be using ReplicaSets. Instead, we would be using something called Deployments. Deployments are preferred in production as they provide more advanced features such as rolling updates, rollbacks and more which allow for easier scaling and management of the application. This makes Deployments help streamline the deployment process and ensure that the deployed application is running smoothly and efficiently.



### StatefulSets

StatefulSet is the controller that manages the deployment and scaling of a set of Stateful pods. A stateful pod in Kubernetes is a pod that requires persistent storage and a stable network identity to maintain its state all the time, even during pod restarts or rescheduling. These pods are commonly used for stateful applications such as databases or distributed file systems as these require a stable identity and persistent storage to maintain data consistency.

- **Unique Identity**: StatefulSets assign each pod a unique index, which provides a consistent and unique identity even if pods are deleted or recreated. This is important for stateful applications that rely on a stable identity to maintain the application state or communicate with other nodes in the cluster.
- **Persistent Network Identit**y: StatefulSets provide each pod with a persistent network identity in the form of a stable hostname based on its ordinal index. This hostname is used for DNS resolution within the cluster, making it easy for other services to discover and communicate with the pods in the StatefulSet, even if they are deleted or recreated.
- **Persistent Storage**: StatefulSets provide persistent storage to their pods through Kubernetes PersistentVolumes, which can be dynamically provisioned and attached to pods as needed. This allows stateful applications to store their data reliably across pod restarts and rescheduling, which is important for applications that need to maintain stateful data, such as databases or distributed file systems.

Hence, in comparison to ReplicaSets or Deployments, which are useful for managing general-purpose tools, StatefulSets are used in managing stateful pods that require a unique identity and stable network identity to maintain their state. Take, for example, a database that requires persistent storage. The database’s nodes would maintain their state so that a new node could take over the previous node’s hostname (unique identity) and network identity and hence make sure that data is consistent.

### DaemonSet

A DaemonSet ensures that a single instance of a pod is running on each node in a cluster. While the earlier controller types ensure that a specific number of replicas are running across the cluster, DaemonSets are intended to run exactly one pod per node. This is particularly useful for running pods as system daemons or background processes that need to run on every node in the cluster. Due to this, DeamonSets can be used for collecting logs, monitoring system performance, and managing network traffic across the entire cluster.

- **Pod Scheduling**: DaemonSets are designed to run one instance of a pod on each node in a cluster. If you need multiple instances, then the other controller types would be more useful.
- **Pod Identity**: similar to StatefulSets, a DaemonSet runs a unique pod on each node as compared to having unique identities for every pod on each node.
- **Rollout Strategy**: since other controller types have multiple instances of a pod, rollouts are done one pod at a time to prevent downtime but as a DaemonSet needs to run a single pod on every node, the rollout process is done one node at a time instead.
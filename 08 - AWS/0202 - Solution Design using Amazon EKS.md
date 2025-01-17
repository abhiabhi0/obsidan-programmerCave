To deploy a containerized application on AWS that requires automatic scaling based on CPU utilization and high availability across multiple Availability Zones, the ideal service to use is **Amazon Elastic Kubernetes Service (EKS)**. Here’s how you would configure it:
### **1. EKS Cluster Setup**
- **Create an EKS Cluster**: Start by creating an EKS cluster that spans multiple Availability Zones (AZs). This ensures high availability and fault tolerance. You can use the AWS Management Console, AWS CLI, or tools like `eksctl` to set this up.
- **Node Groups**: Configure node groups with worker nodes distributed across at least three AZs. This setup allows your application to remain operational even if one AZ experiences issues.

### **2. Autoscaling Configuration**
- **Cluster Autoscaler**: Deploy the Kubernetes Cluster Autoscaler within your EKS cluster. This component automatically adjusts the number of nodes in your cluster based on the resource requests of your pods. It scales up when there are pending pods that cannot be scheduled due to resource constraints and scales down when there are underutilized nodes.
  
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: cluster-autoscaler
    namespace: kube-system
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: cluster-autoscaler
    template:
      metadata:
        labels:
          app: cluster-autoscaler
      spec:
        containers:
        - name: cluster-autoscaler
          image: k8s.gcr.io/cluster-autoscaler:v1.22.0
          args:
          - --cloud-provider=aws
          - --nodes=1:10:<YOUR-NODE-GROUP-NAME>
          - --scale-down-unneeded-time=10m
          - --scale-down-utilization-threshold=0.5
  ```

### **3. Horizontal Pod Autoscaler (HPA)**
- **HPA Configuration**: Implement the Horizontal Pod Autoscaler to automatically scale the number of pods in your deployment based on observed CPU utilization or other select metrics.
  
  ```yaml
  apiVersion: autoscaling/v2beta2
  kind: HorizontalPodAutoscaler
  metadata:
    name: my-app-hpa
    namespace: default
  spec:
    scaleTargetRef:
      apiVersion: apps/v1
      kind: Deployment
      name: my-app-deployment
    minReplicas: 1
    maxReplicas: 10
    metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
  ```

### **4. Security and Access Management**
- **IAM Roles**: Assign IAM roles to your EKS nodes to control access to AWS resources securely.
- **Network Policies**: Implement Kubernetes network policies to control traffic between pods and enhance security.

### **5. Monitoring and Logging**
- Utilize Amazon CloudWatch for monitoring metrics and logs from your EKS cluster, allowing you to set alarms based on specific thresholds for proactive management.

By following these steps, you can ensure that your containerized application is scalable, highly available, and efficiently managed within AWS.

Citations:
[1] https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
[2] https://docs.aws.amazon.com/eks/latest/userguide/autoscaling.html
[3] https://aws.amazon.com/about-aws/whats-new/2024/05/amazon-eks-native-support-autoscaling-coredns-pods/
[4] https://cast.ai/blog/eks-cluster-autoscaler-6-best-practices-for-effective-autoscaling/
[5] https://docs.aws.amazon.com/prescriptive-guidance/latest/scaling-amazon-eks-infrastructure/introduction.html
[6] https://aws.amazon.com/blogs/containers/amazon-eks-control-plane-auto-scaling-enhancements-improve-speed-by-4x/
[7] https://aws.amazon.com/blogs/containers/scaling-amazon-eks-and-cassandra-beyond-1000-nodes/
[8] https://aws.amazon.com/blogs/containers/amazon-eks-cluster-multi-zone-auto-scaling-groups/
[9] https://repost.aws/ko/questions/QUlmqEwKfrQe-clJXvXXCu6w/auto-scale-eks-nodes?sc_ichannel=ha&sc_ilang=en&sc_isite=repost&sc_iplace=hp&sc_icontent=QUlmqEwKfrQe-clJXvXXCu6w&sc_ipos=18
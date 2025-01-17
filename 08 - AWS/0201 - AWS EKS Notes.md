## Overview of AWS EKS

Amazon Elastic Kubernetes Service (EKS) is a fully managed service that simplifies running Kubernetes on AWS without needing to install and operate your own Kubernetes control plane or nodes. EKS provides high availability, scalability, and security for containerized applications.

### Key Features of Amazon EKS
- **Managed Control Plane**: AWS manages the Kubernetes control plane, ensuring high availability across multiple Availability Zones (AZs) and automatically handling scaling and health monitoring of control plane components.
- **Seamless Integration with AWS Services**: EKS integrates with services like IAM for security, CloudWatch for monitoring, VPC for networking, and ECR for container storage.
- **Flexible Compute Options**: Users can run workloads on EC2 instances or use Fargate for serverless compute, allowing for dynamic scaling based on application needs.
- **High Availability and Security**: The service is designed for reliability with automatic updates and patching, along with robust access controls through IAM.

## Basic Concepts

### What is Kubernetes?
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. EKS provides a managed environment to run Kubernetes clusters efficiently.

### Clusters and Nodes
- **Cluster**: A set of EC2 instances (nodes) that run your containerized applications.
- **Node**: An EC2 instance that runs the Kubernetes runtime environment. Nodes can be managed using Amazon EC2 or AWS Fargate.

### Pods
A pod is the smallest deployable unit in Kubernetes, which can contain one or more containers. Pods share networking and storage resources.

## Intermediate Topics

### Networking
EKS uses Amazon VPC to provide networking capabilities. Each EKS cluster runs in a VPC, allowing you to define security groups and network access control lists (ACLs).

### Load Balancing
EKS supports various load balancing options:
- **Application Load Balancer (ALB)**: For HTTP/HTTPS traffic.
- **Network Load Balancer (NLB)**: For TCP traffic.
- **Classic Load Balancer (CLB)**: Legacy load balancing option.

### Security
- **IAM Roles for Service Accounts**: Allows you to assign IAM roles to Kubernetes service accounts to control access to AWS resources.
- **Network Policies**: Define rules for how pods communicate with each other and with other network endpoints.

## Advanced Topics

### Cluster Autoscaler
The Cluster Autoscaler automatically adjusts the size of your cluster based on resource requests. It adds nodes when there are pending pods that cannot be scheduled due to insufficient resources.

### EKS Anywhere
EKS Anywhere allows you to create and operate Kubernetes clusters on your own infrastructure using the same tools as EKS in the cloud, providing flexibility for hybrid deployments.

### CI/CD Integration
EKS integrates seamlessly with CI/CD tools such as Jenkins, GitLab CI/CD, and AWS CodePipeline, enabling automated testing and deployment pipelines for applications.

### Monitoring and Logging
EKS integrates with CloudWatch for logging and monitoring. You can set up alerts based on metrics from your applications running in EKS.

## Use Cases
- **Microservices Architecture**: EKS facilitates deploying microservices where each service can scale independently.
- **Hybrid Cloud Deployments**: Consistent management of workloads across on-premises infrastructure and AWS.
- **High-Performance Computing (HPC)**: Supports compute-intensive workloads using optimized EC2 instances.

## Common Interview Questions

1. **What is Amazon EKS?**
   - Amazon EKS is a fully managed service that makes it easy to run Kubernetes on AWS without managing the control plane.

2. **How does EKS ensure high availability?**
   - EKS runs the control plane across multiple Availability Zones and automatically replaces unhealthy nodes.

3. **What are the main components of an EKS cluster?**
   - The main components include the control plane (managed by AWS), worker nodes (EC2 instances), pods, and services.

4. **What is the difference between EC2 launch type and Fargate in EKS?**
   - EC2 allows users to manage their infrastructure while Fargate provides a serverless option where AWS manages the infrastructure automatically.

5. **How do you secure an EKS cluster?**
   - Security can be managed through IAM roles, network policies, security groups, and encryption at rest/in transit.

6. **What is Cluster Autoscaler?**
   - A feature that automatically adjusts the number of nodes in a cluster based on resource demands.

7. **Can you explain how load balancing works in EKS?**
   - Load balancing distributes incoming application traffic across multiple targets (pods) to ensure no single pod becomes overwhelmed.

8. **What are some best practices for using Amazon EKS?**
   - Use IAM roles for service accounts, implement network policies, enable logging with CloudWatch, and regularly update clusters.

9. **What are some common use cases for Amazon EKS?**
   - Microservices architectures, hybrid cloud deployments, CI/CD automation, edge computing applications, and high-performance computing tasks.

10. **How does EKS integrate with other AWS services?**
    - EKS integrates with IAM for security management, CloudWatch for monitoring/logging, VPC for networking, and ECR for container storage.

By understanding these concepts and preparing answers to common interview questions related to AWS EKS, students will be well-prepared for interviews in cloud technologies focusing on container orchestration with Kubernetes.

Citations:
[1] https://zesty.co/finops-glossary/amazon-kubernetes-eks/
[2] https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
[3] https://www.geeksforgeeks.org/amazon-web-services-introduction-to-amazon-eks/
[4] https://bluexp.netapp.com/blog/aws-cvo-blg-aws-eks-12-key-features-and-4-deployment-options
[5] https://www.amazonaws.cn/en/eks/features/
[6] https://aws.amazon.com/eks/?sc_channel=PS&sc_publisher=google&sc_content=container_e&sc_detail=aws+eks&sc_category=Compute&sc_segment=293617427193&sc_matchtype=e&sc_country=US&s_kwcid=AL%214422%213%21293617427193%21e%21%21g%21%21aws+eks&ef_id=Cj0KCQiAxNnfBRDwARIsAJlH29DGu1BzOClHT6Nek1tJHLR4l_ddClr0jJ6ykNUWC1ANnJWwT3apxW0aApaSEALw_wcB%3AG%3As
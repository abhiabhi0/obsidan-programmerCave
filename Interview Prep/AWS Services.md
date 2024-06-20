### EC2
An [EC2 instance](https://www.geeksforgeeks.org/amazon-ec2-creating-an-elastic-cloud-compute-instance/) stands for Elastic Cloud Compute service, It is a virtual server in the cloud. When we launch this, it will run the selected operating system with a specified application stack. For instance, you can deploy a web server or a database in this EC2 service. It can also be configured for specific computing needs, making it a flexible and scalable solution.

### CloudWatch
CloudWatch helps you to monitor AWS environments like EC2, RDS Instances, and CPU utilization. It also triggers alarms depending on various metrics.

![[Pasted image 20240613112340.png]]

### VPC
VPC stands for Virtual Private Cloud. It allows you to customize your networking configuration. VPC is a network that is logically isolated from other networks in the cloud. It allows you to have your private IP Address range, internet gateways, subnets, and security groups.

### Storage Classes available with Amazon S3 are:
- Amazon S3 Standard
- Amazon S3 Standard-Infrequent Access
- Amazon S3 Reduced Redundancy Storage
- Amazon Glacier

### The three types of EC2 instances based on the costs are:

**On-Demand Instance -** These instances are prepared as and when needed. Whenever you feel the need for a new EC2 instance, you can go ahead and create an on-demand instance. It is cheap for the short-time but not when taken for the long term.

**Spot Instance -** These types of instances can be bought through the bidding model. These are comparatively cheaper than On-Demand Instances.

**Reserved Instance -** On AWS, you can create instances that you can reserve for a year or so. These types of instances are especially useful when you know in advance that you will be needing an instance for the long term. In such cases, you can create a reserved instance and save heavily on costs.

### IAM
AWS IAM enables an administrator to provide granular level access to different users and groups. Different users and user groups may need different levels of access to different resources created. With IAM, you can create roles with specific access-levels and assign the roles to the users. 

It also allows you to provide access to the resources to users and applications without creating the IAM Roles, which is known as Federated Access.

### Connection Draining
Connection Draining is a feature provided by AWS which enables your servers which are either going to be updated or removed, to serve the current requests. 

If Connection Draining is enabled, the Load Balancer will allow an outgoing instance to complete the current requests for a specific period but will not send any new request to it. Without Connection Draining, an outgoing instance will immediately go off and the requests pending on that instance will error out.

### What is an Instance Store Volume and an EBS Volume?
An Instance Store Volume is temporary storage that is used to store the temporary data required by an instance to function. The data is available as long as the instance is running. As soon as the instance is turned off, the Instance Store Volume gets removed and the data gets deleted.

On the other hand, an EBS Volume represents a persistent storage disk. The data stored in an EBS Volume will be available even after the instance is turned off.

**Recovery Time Objective -** It is the maximum acceptable delay between the interruption of service and restoration of service. This translates to an acceptable time window when the service can be unavailable.
**Recover Point Objective -** It is the maximum acceptable amount of time since the last data restore point. It translates to the acceptable amount of data loss which lies between the last recovery point and the interruption of service.

### Is there a way to upload a file that is greater than 100 Megabytes in Amazon S3?
Yes, it is possible by using the Multipart Upload Utility from AWS. With the Multipart Upload Utility, larger files can be uploaded in multiple parts that are uploaded independently. You can also decrease upload time by uploading these parts in parallel. After the upload is done, the parts are merged into a single object or file to create the original file from which the parts were created.

### S3 vs EBS
[S3 ( Simple Storage Service )](https://www.geeksforgeeks.org/introduction-to-aws-simple-storage-service-aws-s3/) is an object storage service suitable for storing various data types of files that can accessed through the internet. 
In contrast, [EBS ( Elastic Block storage )](https://www.geeksforgeeks.org/introduction-to-aws-elastic-block-storeebs/) is a block-level storage attached to EC2 instances, offering persistent and high-performance storage for applications like databases. EBS provides the raw storage hardware helpful for I/O operations where as S3 comes with pre configured file system. For understaning think of S3 as a file storage system and EBS as a hard drive.
S3 is ideal service for storing and retrieving for large amounts of unstructured data such as images and backups. On the other hand EBS is better suitable for databases which are requiring consistent and low-latency performance.

### What Is Elastic Load Balancing (ELB) And How Does It Function?
[Elastic Load balancer ( ELB )](https://www.geeksforgeeks.org/elastic-load-balancer-in-aws/) is a service provided by AWS that helps in distribution of incoming traffic of the applications across multi targets such as EC2 instances, containers etc.. in one or more Availability zones. It helps in improving fault tolerance and ensuring the utilization of resources, bringing high availability of the application by preventing a single node ( instance ) faulterance by improving application’s resilience.

### What Is Amazon RDS, And What Database Engines Does It Support?
Amazon RDS ( Relational Database system) is a managed relational database service that deals with essential hardware infrastructure of Amazon. It supports for the various database engines such as [MySQL](https://www.geeksforgeeks.org/sql-tutorial/), SQL Server, [Oracle](https://www.geeksforgeeks.org/tag/oracle/), PostgreSQL and MariaDB. RDS involves in simplifying the database administration tasks on inclusion of backups, software patching, and scaling. It helps the developers to focus on logic of the application rather than focusing on infrastructure setup and management.

### How Do You Monitor And Log AWS Resources?
AWS comes up with providing services such as [CloudWatch](https://www.geeksforgeeks.org/amazon-web-sevices-introduction-to-amazon-cloudwatch-synthetics/) for monitoring and [CloudTrail](https://www.geeksforgeeks.org/aws-cloudtrail/) for logging. CloudWatch take place in monitoring the resources and applications, while CloudTrail will record the API calls, providing the visibility of user activity. These tools collectively allow detailed observation and analysis of AWS resources.

### What Is The Significance Of Amazon DynamoDB In AWS?
[Amazon DynamoDB](https://www.geeksforgeeks.org/difference-between-amazon-aurora-and-amazon-dynamodb/) is a service in AWS that is helpful in management of NoSQL database service that known for its scalability and low-latency performance. This service is suitable for the applications which requires seamlessly quick access to data, such as gaming, e-commerce, and mobile applications offering consistency of a single-digit millisecond latency.

### Discuss The Benefits Of Using Amazon CloudFront.
[Amazon CloudFront](https://www.geeksforgeeks.org/aws-cloudfront/) is a content delivery network (CDN) service in AWS that speed up the delivery of web content using AWS Global network Infrasture. It enhances the performance, security, and scalability of applications and websites by caching and delivering content from edge locations worldwide. CloudFront also provides additional features including DDoS protection and connecting with other AWS services

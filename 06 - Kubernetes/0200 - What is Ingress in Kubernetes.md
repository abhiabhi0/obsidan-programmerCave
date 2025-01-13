**Kubernetes Ingress** is a powerful API object that manages external access to services within a Kubernetes cluster. It acts as a single entry point for incoming HTTP and HTTPS traffic, enabling efficient routing to various backend services based on defined rules. Here’s a detailed breakdown of its features, functionality, and components.

[[0201 - API Gateway vs Ingress|API Gateway vs Ingress]]
### Key Features of Kubernetes Ingress

- **Routing Rules**: Ingress allows you to define rules that specify how external requests should be routed to different services within the cluster. This can be based on the request's hostname or URL path.

- **Single Entry Point**: Instead of exposing each service with its own load balancer, Ingress consolidates access through a single IP address and port, simplifying management and reducing resource usage.

- **Load Balancing**: Ingress can distribute incoming traffic across multiple instances of a service, enhancing availability and performance.

- **SSL/TLS Termination**: It can handle SSL termination, meaning it can manage secure connections by decrypting incoming HTTPS traffic before it reaches the backend services. This centralizes security management.

- **Name-based Virtual Hosting**: Ingress supports hosting multiple services under the same IP address by routing requests based on the hostname.

### How Ingress Works

1. **Ingress Resource**: You define an Ingress resource in your Kubernetes configuration, specifying the routing rules for incoming traffic. This includes the hostnames and paths that should direct traffic to specific services.

2. **Ingress Controller**: An Ingress resource alone does not process traffic; it requires an **Ingress Controller**. The controller is responsible for implementing the rules defined in the Ingress resource. Popular controllers include NGINX, Traefik, and HAProxy.

3. **Traffic Flow**:
   - External clients send requests to the Ingress IP.
   - The Ingress Controller receives these requests and evaluates them against the defined rules.
   - Based on matching criteria (host/path), the controller routes the requests to the appropriate backend service (which consists of Pods).

### How Ingress Controllers Work

- **Ingress Controller**: An Ingress Controller is a component that implements the rules defined in the Ingress resource. It acts as a reverse proxy, managing incoming requests and directing them to the appropriate backend services based on the defined routing rules.
- **Operation**:
    
    1. The user defines an Ingress resource with specific routing rules.
    2. The Ingress Controller watches for changes in Ingress resources.
    3. Upon detecting changes, it updates its configuration to route traffic according to the specified rules.
    
- **Popular Ingress Controllers**:
    
    - NGINX Ingress Controller
    - Traefik
    - HAProxy

### Example Use Case

Suppose you have two services running in your cluster:
- An API service accessible at `api.example.com`
- A web application accessible at `www.example.com`

You can set up an Ingress resource to route traffic as follows:
- Requests to `api.example.com` are directed to the API service.
- Requests to `www.example.com` are directed to the web application service.

### Summary

Kubernetes Ingress is essential for managing external access to services within a cluster efficiently. By providing a centralized mechanism for routing HTTP/S traffic, it simplifies application deployment and enhances security through features like SSL termination and load balancing. This makes it particularly valuable in production environments where multiple services need to be exposed securely and efficiently without overwhelming resources with numerous load balancers.

### Diagram of Ingress Architecture

Here’s a simplified representation of how Kubernetes Ingress fits into the architecture:

```
+-------------------+
| External Traffic  |
| (HTTP/HTTPS)     |
+-------------------+
         |
         v
+-------------------+
|    Ingress        |
|   (API Object)    |
+-------------------+
         |
         v
+-------------------+
| Ingress Controller |
| (e.g., NGINX)     |
+-------------------+
         |
         v
+-------------------+
|      Services     |
|                   |
| +---------------+ |
| | Service A     | |<---> Pod A1
| +---------------+ |<---> Pod A2
|                   |
| +---------------+ |
| | Service B     | |<---> Pod B1
| +---------------+ |<---> Pod B2
+-------------------+
```

In this diagram:
- External traffic flows into the cluster through the Ingress.
- The Ingress Controller processes this traffic according to defined rules.
- Requests are routed to appropriate services, which manage their respective Pods.

Citations:
[1] https://konghq.com/blog/learning-center/what-is-kubernetes-ingress
[2] https://www.solo.io/topics/kubernetes-api-gateway/kubernetes-ingress
[3] https://www.geeksforgeeks.org/what-is-kubernetes-ingress/
[4] https://www.bmc.com/blogs/kubernetes-ingress/
[5] https://traefik.io/glossary/kubernetes-ingress-and-ingress-controller-101/
[6] https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html
[7] https://www.armosec.io/glossary/kubernetes-ingress/
[8] https://www.getambassador.io/resources/kubernetes-ingress
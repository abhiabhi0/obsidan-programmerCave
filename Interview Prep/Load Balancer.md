#### **What is a Load Balancer?**

A load balancer is a device that distributes network or application traffic across multiple servers to ensure scalability, reliability, and performance. It acts as the entry point of an application, receiving all incoming traffic and managing it effectively.

---

#### **Concepts**

- **Placement**: Load balancers typically sit at the edge of data centers, balancing traffic across servers.
- **Functionality**:
    - Distributes incoming traffic to servers.
    - Handles responses from servers and forwards them to clients.
    - Shields server details by proxying requests.

---

#### **Why Use a Load Balancer?**

1. **Scalability**:
    
    - Handles traffic spikes by distributing requests across multiple servers.
    - Allows horizontal scaling by adding/removing servers.
2. **Resilience**:
    
    - Implements health checks to ensure only active servers handle traffic.
    - Dynamically adjusts the number of servers based on traffic thresholds.
3. **Higher Availability**:
    
    - Reduces downtime during deployments by staggering server updates (rolling deployment).
    - Automatically reroutes traffic if a server becomes unhealthy.
4. **Security**:
    
    - Acts as a barrier, protecting server IPs from exposure.
    - Can identify and block malicious requests (e.g., DDoS attacks).
5. **Performance**:
    
    - Offloads SSL/TLS encryption and decryption tasks from servers.
    - Compresses traffic to enhance efficiency.

---

#### **Deployment Process with Load Balancer**

1. Detach a host from the load balancer.
2. Deploy new application code to the host.
3. Restart the application and perform sanity checks.
4. Reattach the host to the load balancer.
5. Repeat for all hosts to avoid downtime.

---

#### **How Load Balancers Work**

1. **Reverse Proxy**:
    
    - Accepts client requests, forwards them to servers, and sends back responses.
    - Hides server details by including only the proxy server's information in headers.
2. **Health Checks**:
    
    - Regularly pings servers to ensure they are active.
    - Marks unhealthy servers as inactive.
    - Monitors inactive servers to re-enable them if they recover.
3. **Self-Scaling**:
    
    - Can dynamically scale based on traffic demands (e.g., AWS load balancers scale up to 100 nodes).

---

#### **Load Balancing Algorithms**

- **Round Robin**: Distributes client requests to servers in sequential order.
- **Other Algorithms (not detailed)**: Weighted, least connections, and IP hash.
- [[Types of load balancing algorithms]]

---

#### **Key Components of Load Balancer Configuration**

1. **Listeners**:
    
    - Defines the protocols and ports for incoming traffic (e.g., HTTP on port 80, HTTPS on port 443).
    - Specifies the destination port on the server (e.g., port 8080).
2. **Routing**:
    
    - Allows conditional forwarding (e.g., admin requests go to admin servers).
3. **Target Groups**:
    
    - Grouping of servers based on request types.
    - Enables specialized handling (e.g., static vs. dynamic content).

---

#### **Types of Load Balancers**

1. **Application Load Balancers**:
    
    - Operate at the application layer (HTTP/HTTPS).
2. **DNS Load Balancers**:
    
    - Distribute traffic by resolving DNS requests to different server IPs.
3. **Network Load Balancers**:
    
    - Operate at the transport layer (TCP/UDP).

---

#### **Further Reading and Questions**

1. **Global Server Load Balancer (GSLB)**:
    
    - How is traffic across regions regulated?
    - Deployment strategies for global traffic distribution.
2. **Session Stickiness**:
    
    - How to handle scenarios where a user switches between servers during a session?
3. **Static and Dynamic Content**:
    
    - Optimizing architecture for static and dynamic content delivery.
4. **Configuration Examples**:
    
    - Refer to [NGINX Load Balancer Documentation](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/) for configuration details.

- **Platform Overview**:
    - All-in-one platform for Kubernetes automation, optimization, security, and cost management.
    - Abstracts provider-specific technical complexity, simplifying Kubernetes operations across major cloud providers.
    
- **Cost Management**:
    - Real-time and longer-period cost monitoring at cluster, namespace, and workload levels.
    - Offers cost optimization suggestions and automatic optimization through features like autoscaling, spot instance automation, and bin packing.
    
- **Security**:
    - Checks cluster security configuration for misconfigurations and vulnerabilities.
    - Automatically prioritizes fixes to improve security posture.
    - Provides scanning against industry standards such as CIS Benchmarks.

### **Connect your cluster**

this step will require installing a [read-only agent](https://docs.cast.ai/docs/about-the-read-only-agent) in your terminal or cloud shell. You will be guided through the process in the console.

### CAST AI Agent

- Connects Kubernetes cluster to CAST AI platform for automation, cost monitoring, and optimization.
- Operates on read-only principle, ensuring minimal intrusion and requiring explicit permission for configuration changes.
- Accesses minimal cluster data for insights on resource utilization (storage, memory, CPU).
- Accessible resources include nodes, pods, deployments, stateful sets, daemon sets, and environment variables
- Does not access sensitive user data such as secrets, config maps, or sensitive environment variables.
- Filters out sensitive environment variables (e.g., containing passwords, tokens, keys) before analysis.
- Cannot view or access the contents of sensitive Kubernetes resources.
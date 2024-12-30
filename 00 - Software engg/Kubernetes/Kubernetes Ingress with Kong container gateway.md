To provide public access to microservices running inside a Kubernetes cluster using the Kong API Gateway and Kubernetes Ingress, you'll need to follow these steps:

### Step 1: Deploy Kubernetes Cluster
Ensure you have a running Kubernetes cluster. You can set up a local cluster using Minikube, or use a managed Kubernetes service like AWS EKS, Google Kubernetes Engine (GKE), or Azure Kubernetes Service (AKS).

### Step 2: Install Kong Ingress Controller
Deploy the Kong Ingress Controller to your Kubernetes cluster. You can do this using Helm, which simplifies the installation and management of Kubernetes applications.

1. **Add the Helm repository for Kong:**

   ```sh
   helm repo add kong https://charts.konghq.com
   helm repo update
   ```

2. **Install the Kong Ingress Controller:**

   ```sh
   helm install kong/kong --name kong --namespace kong --set ingressController.installCRDs=false
   ```

   This command will install Kong and the Ingress Controller in the `kong` namespace.

### Step 3: Deploy Your Microservices
Deploy your microservices to the Kubernetes cluster. Each microservice should be exposed internally using a Kubernetes `Service` resource.

Example YAML file for a microservice:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: example-service
spec:
  selector:
    app: example
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
        - name: example-container
          image: your-docker-image
          ports:
            - containerPort: 8080
```

### Step 4: Create Ingress Resources
Create Ingress resources to define how requests should be routed to your services. The Ingress resource will use Kong as the controller.

Example Ingress resource for the `example-service`:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: default
  annotations:
    konghq.com/strip-path: "true"
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: example-service
            port:
              number: 80
```

### Step 5: Configure DNS and SSL (Optional)
Configure DNS to point to your Kubernetes cluster's external IP or load balancer. If you want to use HTTPS, configure SSL certificates using Kubernetes Secrets and update the Ingress resource to use `tls`.

Example with TLS:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: default
  annotations:
    kubernetes.io/ingress.class: "kong"
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
spec:
  tls:
  - hosts:
    - example.com
    secretName: example-tls
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: example-service
            port:
              number: 80
```

### Step 6: Verify the Setup
1. **Check Ingress Controller Status:**
   Ensure that the Kong Ingress Controller is running correctly.

   ```sh
   kubectl get pods -n kong
   ```

2. **Check Ingress Resource:**
   Verify that the Ingress resource is created and has the correct rules.

   ```sh
   kubectl get ingress
   ```

3. **Test Access:**
   Access your service using the hostname specified in the Ingress resource (e.g., `http://example.com`).

### Conclusion
By following these steps, you've set up Kubernetes Ingress with Kong as the API Gateway to provide public access to your microservices. This setup leverages Kong's powerful features for routing, load balancing, and securing your microservices, while using Kubernetes Ingress resources to define the routing rules.
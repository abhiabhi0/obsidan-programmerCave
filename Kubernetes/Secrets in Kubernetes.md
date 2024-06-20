- Secrets in Kubernetes are objects that contain sensitive data such as passwords, tokens, or keys. 
- They can be created independently of Pods, reducing the risk of exposure during the workflow of creating, viewing, and editing Pods. 
- Kubernetes Secrets are, by default, stored unencrypted in the API server's data store (etcd). 
- To safely use Secrets, take steps such as encrypting them and limiting API access. 
- Secrets can be used for various purposes, such as: 
 * Storing dotfiles in a secret volume. 
 * Allowing one container in a Pod to access a private key, while another container cannot. 

- The AWS Secrets Manager secrets can be used in Amazon Elastic Kubernetes Service (EKS) by using the AWS Secrets and Configuration Provider (ASCP) with the Kubernetes Secrets Store CSI Driver.
- Secrets in Secrets Manager can be retrieved as files in EKS pods. 
- You can choose which key/value pairs to mount. 
- The ASCP uses JMESPath syntax to query key/value pairs in your secret. 
- It also works with AWS Systems Manager Parameter Store. 
- If using a private EKS cluster, ensure the VPC has a Secrets Manager endpoint for the Secrets Store CSI Driver to make calls to Secrets Manager. 
- If using Secrets Manager automatic rotation, the Secrets Store CSI Driver rotation reconciler feature can ensure you are retrieving the latest secret. 

###  To grant your EKS pod access to secrets in Secrets Manager: 
- Create a permissions policy for the required secrets. 
- Create an IAM OpenID Connect (OIDC) provider for the cluster. 
- Create an IAM role for the service account and attach the policy to it. 
- To install and configure the ASCP: 
- Obtain it from the secrets-store-csi-provider-aws repository on GitHub. 
- Install and configure it using Helm or YAML files provided in the repo. 
- To mount secrets in EKS: 
- Create a SecretProviderClass YAML file listing the secrets and the file name to mount them as. 
- Apply the SecretProviderClass to the pod. 
- Deploy your pod.
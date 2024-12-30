1. Kubernetes objects are persistent entities that represent the state of a Kubernetes cluster. 
2. Objects can be created, modified, or deleted using the Kubernetes API. 
3. Almost every Kubernetes object includes two nested object fields: `spec` and `status`. 
4. `spec` describes the desired state of an object, while `status` describes the current state of an object. 
5. Kubernetes actively works to ensure that the actual state of an object matches the desired state. 
6. Objects are created in Kubernetes by providing a manifest, a file that includes the object spec and basic information about the object. 
7. Manifests are typically written in YAML or JSON format.
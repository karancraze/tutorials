# Kubernetes (k8s)

### k8s is a system to deploy containerized apps
---
## Tl;dr description
Nodes are individual machines that run containers  
Masters are machines with set of program that manages all nodes  
k8s does not build images it just copies and runs them  
k8s master decides where to run each container  
To deploy, we update desired state of the master with a config file  
Master works constantly to meet the requirements of the desired state  

### K8s supports two approaches -
#### Imperative Deployments (Difficult and manual approach)
- List all running instances
- Manually Identify image versions of the instances
- Figure out a strategy for updating/redeploying instances with new image
#### Declarative Deployments (Best practice and automatically handles redployments)
- Update config file and send to kubectl
- k8s master node will handle care of it

##### Note: Declarative is better. Some blogs suggest imperative approach ignore them

---
## Development Environment
#### minikube
Used in dev environment  
Set up tiny kubernetes cluster on local machine inside a VM  
Installation -
  - Install - ```brew cask install minikube```
  - Checking installation - ```which minikube```
  - Start the cluster - ```minikube start```
  - Get ip address - ```minikube ip```
 - alias docker inside minikube to docker cli - ```eval $(minikube docker-env)```
---
## Production Environment
Use managed service like
- (EKS) Amazon Elastic Container Service for Kubernetes
- (GKE) Google Cloud Kubernetes Engine

---
## kubectl - (Orchestrator)
For managing Nodes locally or in production  

**Requires brew on MacOS - brew.sh**  
**Install virtualbox from virtualbox.org**

Installation
- ```brew install kubectl```

Usage:
- Creating an object instance
    - ```kubectl apply -f <filename>```  
    - ```apply``` - change current configuration of cluster
    - ```-f``` filename - path to file with config

- Creating an instance
    - ```kubectl delete -f <filename>```
    - ```delete``` - delete the object instance

- Getting status of all objects with the specified type in the cluster  
    ```kubectl get <type>``` optinal ```-o wide```  
    ```get``` - retrieve info about a running object  
    ```<type>```
    - ```pods``` - type of object instance
    - ```services``` - print all services, does not print targetPort
    - ```-o wide``` - gives ip address and node

- Getting complete description of object instances
    - ```kubectl describe <type>```

- Updating a property on existing config
    - ```kubectl set <property> <object_type>/<object_name> = <new value for property>```


---

## Kubernetes Terminology

### kube-proxy
- Every single node has program of kube-proxy
- One single window to the outside window
- Manages flow - Inspects requests and routes it to different services or nodes

---
### selectors (label selector system)
nodes are linked together by labels  
labels are key value pairs  
Custom coded and must match on source and destination nodes  
handles routing  

---
### ports
port
- port which another pod/client/node can access
- when you have mulitple services

targetPort
- Incomming traffic is routed to other container by this port
- Should be identical to container port the other container

nodePort
- port which is exposed for testing/development
- 30000-32767
- If not specified it'll be randomly selected
- Is not used in production environment

---
## Configuration File
#### Name and Kind are unique identifier tokens for running instances
#### If new name is defined in config, it'll create new instances for it instead of updating the current instance with new config

### Kind
---
#### - Pod (Not used in production)
Smallest thing that we can deploy

For deploying containers we use pods

One or more closely related containers



#### - Service
Setup networking and routing inside kubernetes cluster 

Sub types
- ClusterIP 
- NodePort
    - Expose container to outside world. For dev purpose only
    - Never use on prodcution except for exceptional use cases
- LoadBalancer
- Ingress


#### - Deployment (Used in Dev as well as production)
Maintains set of identical pods
Ensure they have correct config
- name - Name of this deployment
- replicas - Number of replicas to create
- selector - Notifies master to create instances with this selectors
- template - Structure of the instance to create
---

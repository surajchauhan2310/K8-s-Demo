Kubernetes Commands and Workflow
This document outlines the key commands and steps executed to manage Kubernetes resources such as Pods, ReplicaSets, and ReplicationControllers. It also includes explanations and examples for better understanding.

Table of Contents
Setting Up Minikube

Managing Pods

Working with Labels and Selectors

ReplicaSet Management

ReplicationController Management

Node Selectors and Taints

Troubleshooting and Debugging

Setting Up Minikube
Start Minikube to create a local Kubernetes cluster:

bash
Copy
minikube start
Check Minikube status:

bash
Copy
minikube status
Verify nodes in the cluster:

bash
Copy
kubectl get nodes
Describe a specific node (e.g., minikube):

bash
Copy
kubectl describe node minikube
Managing Pods
Create a Pod
Save the following YAML as pod1.yml:

yaml
Copy
apiVersion: v1
kind: Pod
metadata:
  name: testpod3
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-World; sleep 5; done"]
Apply the Pod configuration:

bash
Copy
kubectl apply -f pod1.yml
Check Pod Status
List all Pods:

bash
Copy
kubectl get pods
Get detailed information about a Pod:

bash
Copy
kubectl describe pod testpod3
View Pod Logs
Stream logs from a container in the Pod:

bash
Copy
kubectl logs -f testpod3 -c c00
Delete a Pod
Remove a specific Pod:

bash
Copy
kubectl delete pod testpod3
Working with Labels and Selectors
Add Labels to Pods
Label a Pod (e.g., delhi-pod):

bash
Copy
kubectl label pod delhi-pod zone=north
List Pods with labels:

bash
Copy
kubectl get pods --show-labels
Filter Pods Using Labels
Get Pods where env is not production:

bash
Copy
kubectl get pods -l 'env notin(production)'
Get Pods where env is either production or development:

bash
Copy
kubectl get pods -l 'env in(production,development)'
ReplicaSet Management
Create a ReplicaSet
Save the following YAML as replica-set.yml:

yaml
Copy
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myrs
spec:
  replicas: 2
  selector:
    matchLabels:
      myname: Suraj
  template:
    metadata:
      labels:
        myname: Suraj
    spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Hello-Suraj; sleep 5; done"]
Apply the ReplicaSet configuration:

bash
Copy
kubectl apply -f replica-set.yml
Scale a ReplicaSet
Increase the number of replicas to 8:

bash
Copy
kubectl scale --replicas=8 rs/myrs
Delete a ReplicaSet
Remove the ReplicaSet and its Pods:

bash
Copy
kubectl delete rs myrs
ReplicationController Management
Create a ReplicationController
Save the following YAML as replica-controller.yml:

yaml
Copy
apiVersion: v1
kind: ReplicationController
metadata:
  name: myreplica
spec:
  replicas: 2
  selector:
    myname: Suraj
  template:
    metadata:
      labels:
        myname: Suraj
    spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Hello-Suraj; sleep 5; done"]
Apply the ReplicationController configuration:

bash
Copy
kubectl apply -f replica-controller.yml
Scale a ReplicationController
Increase the number of replicas to 10:

bash
Copy
kubectl scale --replicas=10 rc -l myname=Suraj
Delete a ReplicationController
Remove the ReplicationController and its Pods:

bash
Copy
kubectl delete rc myreplica
Node Selectors and Taints
Label a Node
Add a label to a node (e.g., minikube):

bash
Copy
kubectl label nodes minikube hardware=t2-medium
Create a Pod with Node Selector
Save the following YAML as node-selector.yml:

yaml
Copy
apiVersion: v1
kind: Pod
metadata:
  name: nodelabels
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-Node; sleep 5; done"]
  nodeSelector:
    hardware: t2-medium
Apply the Pod configuration:

bash
Copy
kubectl apply -f node-selector.yml
Troubleshooting and Debugging
Describe a Resource
Get detailed information about a resource (e.g., Pod, ReplicaSet):

bash
Copy
kubectl describe pod <pod-name>
kubectl describe rs <replicaset-name>
Exec into a Pod
Open a shell inside a container:

bash
Copy
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash
Check Service and Pod Connectivity
Get Pod IP and test connectivity:

bash
Copy
kubectl get pods -o wide
curl http://<pod-ip>:<port>

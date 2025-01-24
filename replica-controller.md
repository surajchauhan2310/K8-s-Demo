Kubernetes ReplicationController Management
This document outlines the steps to manage a ReplicationController (RC) and associated pods in a Kubernetes cluster. It also includes the YAML configuration for reference.

ReplicationController YAML Configuration
Below is the replica-controller.yml file used to create and manage the ReplicationController:

yaml
Copy
kind: ReplicationController
apiVersion: v1
metadata:
  name: myreplica
spec:
  replicas: 2
  selector:
    myname: Suraj  # Label selector updated to match the new name
  template:
    metadata:
      name: testpod6
      labels:
        myname: Suraj  # Label updated to match the selector
    spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Hello-Suraj; sleep 5 ; done"]
Key Commands Executed
1. Create or Update the ReplicationController
Apply the ReplicationController configuration defined in the YAML file:

bash
Copy
kubectl apply -f replica-controller.yml
2. Check Running Pods
List all running pods:

bash
Copy
kubectl get pods
3. Check Pods with Labels
View pods along with their labels:

bash
Copy
kubectl get pods --show-labels
4. Scale the ReplicationController
Scale the ReplicationController to 10 replicas using a label selector:

bash
Copy
kubectl scale --replicas=10 rc -l myname=Suraj
5. Verify Scaling
Check the pods after scaling to ensure the desired number of replicas are running:

bash
Copy
kubectl get pods --show-labels
6. Describe the ReplicationController
Get detailed information about the ReplicationController:

bash
Copy
kubectl describe rc
7. Scale Down the ReplicationController
Reduce the number of replicas to 1:

bash
Copy
kubectl scale --replicas=1 rc -l myname=Suraj
8. Delete the ReplicationController
Delete the ReplicationController and its associated pods:

bash
Copy
kubectl delete rc myreplica
9. Verify Deletion
Confirm that the ReplicationController and its pods have been deleted:

bash
Copy
kubectl get rc
kubectl get pods

# Kubernetes notes

## Pods
Pods are the atomic unit of a Kubernetes cluster. A pod is a collection of containers and other resources. The most common type of pod is a single-container pod. 
  
### Pod contents
  - single container
  - multi container
  - init container (verifies conditions before starting containers)

### Pod Features
  - Storage (volumes attached to pod, available to all containers)
  - Networking

## Commands
`k run nginx-yaml --image=nginx --dry-run=client -o yaml`
Generates the yaml of a pod that would be sent to the cluster without sending it. Good for creating yaml template of a pod quickly.

`k run nginx-yaml --image=nginx --dry-run=server -o yaml`
Generates the yaml of a pod, sends it to the cluster to initiate it, and then deletes it

`k create -f nginx.yaml`
Only creates pod from yaml file

`k apply -f nginx.yaml`
Creates or applies changes to pod from yaml file

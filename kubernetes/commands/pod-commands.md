# Pod Commands

### Create a new pod
`kubectl run podname --image=nginx`

### Get all pods across all namespaces
`kubectl get pods -A`

### Describe running pod
`kubectl describe pod podname`

### Get yaml of running pod to reveal config
`kubectl get pod podname -o yaml`

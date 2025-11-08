# ReplicaSet Commands

### Create ReplicaSet from file
`kubectl create -f replicaset-definition.yaml`

### Get ReplicaSets
`kubectl get replicaset`

### Delete ReplicaSets
`kubectl delete replicaset myapp-replicaset`

### Replace / Update ReplicaSets
`kubectl replace -f replicaset-definition.yaml`

### Scale ReplicaSet (update replicas count, does not change file though)
`kubectl scale -replicas=6 -f replicaset-definition.yaml`

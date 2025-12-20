
### imperatively create a ClusterRole

```
kubectl create clusterrole michelle-role --verb=get,list,watch,create --resource=nodes
```

### Imperatively create a ClusterRole with multiple resources
```
kubectl create clusterrole storage-admin --resource=persistentvolumes,storageclasses --verb=list,create,get,watch
```
### Imperatively create a ClusterRoleBinding
```
kubectl create clusterrolebinding michelle-role-binding --clusterrole=michelle-role --user=michelle
```

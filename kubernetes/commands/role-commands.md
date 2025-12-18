
### Get roles
```
kubectl get roles
```

### Get role bindings
```
kubectl get rolebindings
```

### Create role imperatively
```
kubectl create role developer --verb=list,create,delete --resource=pods
```

### Create rolebinding imperatively
```
kubectl create rolebinding dev-user-binding --role developer --user dev-user
```

### Edit role imperatively
```
kubectl edit role developer
```

### Get information about 'developer' role
```
kubectl describe role developer
```

### Get information about 'dev-user-developer-binding' rolebinding
```
kubectl describe rolebinding dev-user-developer-binding
```

### Check my own permissions to 'create deployments' or 'delete nodes'
```
kubectl auth can-i create deployments
kubectl auth can-i delete nodes
```

### As admin, check what dev-user can do
```
kubectl auth can-i create deployments --as dev-user
kubectl auth can-i create deployments --as dev-user --namespace test
kubectl auth can-i get pod/dark-blue-app --as dev-user --namespace blue
kubectl get pods --as dev-user
```


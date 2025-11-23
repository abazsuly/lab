# Log Commands

### Get container logs
```
kubectl logs myapp-pod
```

### Get initContainer logs
```
kubectl logs myapp-pod -c init-myservice # inspect initcontainer logs
```

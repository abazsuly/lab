# ConfigMap Commands

### Imperatively create ConfigMap
```
kubectl create configmap myconfigmap \
    --from-literal=APP_COLOR=blue \
    --from-literal=APP_MOD=prod
```

### Create ConfigMap from file
```
kubectl create configmap app-config \
    --from-file=app_config.properties
```

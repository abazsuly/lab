# Secrets Commands

### Imperative command from-literal
```
kubectl create secret generic <secret-name> \
   --from-literal=<key>=<value>
```
```
kubectl create secret generic app-secret \
   --from-literal=DB_host=mysql \
   --from-literal=DB_user=root \
   --from-literal=DB_password=password \
```
### Imperative command from-file
```
kubectl create secret generic <secret-name> \
   --from-file=<path-to-file>
```
```
kubectl create secret generic app-secret \
   --from-file=app_secret.properties
```

### Secret file 'app_secret.properties'
```
DB_host: mysql
DB_user: root
DB_password: password
```


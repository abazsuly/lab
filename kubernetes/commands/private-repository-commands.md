
### To create a secret so that a pod can pull from a private repository:
```
kubectl create secret docker-registry regcred \
  --docker-server= private-registry.io \
  --docker-username= registry-user \
  --docker-password= registry-password \ 
  --docker-email= registry-user@org.com 
```

In the pod definition, specify the secret to pull the image via `imagePullSecrets`:
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: private-registry.io/apps/internal-app
  imagePullSecrets:
  - name: regcred
```


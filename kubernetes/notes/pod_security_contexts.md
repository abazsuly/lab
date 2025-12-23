
To set a securityContext on a pod:
```
apiVersion: v1
metadata:
  name: web-pod
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
```

To set a securityContext on a container:
```
apiVersion: v1
metadata:
  name: web-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities:
          add: ['MAC_ADMIN']
```

Notes:
1. Settings on the pod are inherited by all containers inside the pod.
2. Settings on the container will override settings on the pod.
3. Capabilities can only be added at the container level
4. If `runAsUser` is not set, the default user in the container is root (container root, not host root)
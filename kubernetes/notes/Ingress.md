
## Ingress Notes
Ingress represents the method to provide HTTP(S) entry point into the cluster.

Ingress standardizes Layer 7 HTTP/S routing.
- Host-based routing
- Path-based routing
- HTTP semantics (headers, methods, status codes)
- TLS termination
### Ingress Components
Ingress Controller requires:
1. Ingress deployment
2. Service (NodePort, example 80->80, 443->443)
3. ConfigMap (SSL authentication params, etc)
4. ServiceAccount (Auth)
### Ingress Rules
1. Rules control what gets served to domains and pathing
	1. i.e. my-online-store.com -> main app
	2. watch.my-online-store.com -> video app
	3. my-online-store.com/watch -> video app (different path)
	4. Incorrect paths and subdomains get sent to 404 page
	
### Ingress Resource
Base case:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear
spec:
  defaultBackend: 
    name: wear-service
    port: 
      number: 80
```

With rules routing traffic from path to service:
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
    - http:
        paths:
        - path: /wear
          pathType: Prefix
          backend:
            service:
              name: wear-service
              port: 
                number: 80
        - path: /watch
          pathType: Prefix
          backend:
            service:
              name: watch-service
              port: 
                number: 80
          
```

With rules routing traffic via hostnames. 
An annotation is provided to 'erase' path /watch from arriving at the service. The service would serve at '/' and not '/pay', but we want '/pay' to arrive at service root path. This is an NGINX feature.
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wear-watch
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: wear.my-online-store.com
      http:
        paths:
        - path: /wear
          pathType: Prefix
          backend:
            service:
              name: wear-service
              port: 
                number: 80
    - host: watch.my-online-store.com
      http:
        - path: /watch
          pathType: Prefix
          backend:
            service:
              name: watch-service
              port: 
                number: 80
          
```
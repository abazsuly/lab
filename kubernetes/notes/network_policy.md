### Network Policy
A resource that is linked and applied to a pod. 
A NetworkPolicy applies basic firewall rules on a pod to pod basis. 
Not all network solutions used by Kubernetes support Network Policies:
```
Support Network Policies:
1. kube-router
2. Calico
3. Romana

Does not support Network Policies:
1. Flannel
```


### NetworkPolicy definition
```yml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
    namespaceSelector:
      matchLabels:
        kubernetes.io/metadata.name: prod
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 3306
  egress:
  - to: 
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
      - protocol: TCP
        port: 80
```


NetworkPolicy notes:
1. By default, pods are wide open to communicating with other pods. Once a NetworkPolicy is defined, then they become default deny except for specified ingress/egress. An empty `from:` or `to:` means "allow nothing".
2. Ingress and egress criteria are defined via `podSelector` and `namespaceSelector` arguments.
3. Policies are namespace scoped. 
4. In the above definition, `podSelector` and `namespaceSelector` are linked and function as an "and" definition. Both selectors must match the incoming traffic for it to be permitted (incoming traffic must come from api-pods inside the 'prod' namespace). Adding a dash to `namespaceSelector` would unlink it from the `podSelector` rule and would function as an "or" with regard to both, if either are true then the traffic is permitted (api-pods can connect, and also anything in the 'prod' namespace could connect).
5. The `ipBlock` selector functions to permit traffic to external resources via their IP
6. The `ports:` definition lives at the same indentation level of `from:` and `to:` as they refer to where those definitions can go.

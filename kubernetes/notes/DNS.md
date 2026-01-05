## DNS Notes

1. Within the same namespace, using Kube DNS, a pod is resolved to its name
```
curl http://web-service
Welcome to NGINX!
```
2. Within a different namespace, using Kube DNS, a pod is resolved to its name + namespace
```
curl http://web-service.apps
Welcome to NGINX!
```
3. Within a different namespace, using Kube DNS, and with a Service resource deployed,  a pod is resolved to its name + namespace + 'svc'. This creates a less ambiguous and more canonical reference to the domain
```
curl http://web-service.apps.svc
Welcome to NGINX!
```
4. For a perfectly canonical reference to a Kube managed DNS resolution (i.e. not to be confused with any other external DNS resolver and their responses), default subdomain 'cluster.local' is added to the end. This name can be changed during cluster provisioning but defaults to this name if not specified.
```
curl http://web-service.apps.svc.cluster.local
Welcome to NGINX!
```
5. Cross-cluster resolution is possible but generally speaking the cluster boundary is preserved. Example: `*.cluster1.local` and `*.cluster2.local`. Cross-cluster DNS requires additional external configurations as each cluster has its own hard DNS and network boundary.
### Summary:
```
cluster.local  → “this is Kubernetes DNS”
svc            → “this is a Service”
namespace      → “where it lives”
name           → “what I want”
```

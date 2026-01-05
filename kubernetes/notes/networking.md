
# Networking Notes

## Commonly Used Ports in Kubernetes
| Application             | Port        |
| ----------------------- | ----------- |
| etcd                    | 2379        |
| kube-api                | 6443        |
| kubelet                 | 10250       |
| kube-controller-manager | 10257       |
| kube-scheduler          | 10259       |
| etcd peer communication | 2380        |
| NodePort Services       | 30000-32767 |
## Networking CNI Plugins
#### Location of CNI plugins
```
/opt/cni/bin
```
#### Location of CNI plugin configs
```
/etc/cni/net.d
```

#### Notes
1. The first valid config in alphabetical order in `/etc/cni/net.d` is used by the container runtime to execute networking configuration.



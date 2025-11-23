# Upgrade kubeadm Cluster Commands

Target new version: 1.34.0-1.1

## Guidelines
1. Start with controlplane first before moving on to other nodes.
2. Drain node before proceeding with upgrade

## Upgrade ControlPlane

### Check current repository for latest version
```
cat /etc/apt/sources.list.d/kubernetes.list
```

### Update sources line to use new version (i.e. replace 1.33 with 1.34 in the line)
```
vim /etc/apt/sources.list.d/kubernetes.list
```

### Update packages
```
sudo apt update
sudo apt-cache madison kubeadm
```

### upgrade kubeadm (replace command with latest version string)
```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.34.0-1.1' && \
sudo apt-mark hold kubeadm
```

### Check kubeadm version and verify upgrade plan
```
kubeadm version
sudo kubeadm upgrade plan
```

### Drain the target node
```
kubectl drain controlplane --ignore-daemonsets
```

### Run component upgrade
```
sudo kubeadm upgrade apply v1.34.0
```

### Check kubeadm version 
```
kubeadm version
```

### Upgrade kubelet after updating version string in command
```
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.34.0-1.1' kubectl='1.34.0-1.1' && \
sudo apt-mark hold kubelet kubectl
```

### Restart kubelet
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

### Uncordon the node
```
kubectl uncordon controlplane
```

## Upgrade worker nodes

### Upgrade kubeadm (supply new version string)
```
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.34.0-1.1' && \
sudo apt-mark hold kubeadm
```

### Upgrade node
```
sudo kubeadm upgrade node
```

### Drain the node (executed from controlplane)
```
kubectl drain node01 --ignore-daemonsets
```

### Upgrade kubelet and kubectl
```
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.34.0-1.1' kubectl='1.34.0-1.1' && \
sudo apt-mark hold kubelet kubectl
```

### Reload kubelet
```
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```

### Uncordon node (from control plane)
```
kubectl uncordon node01
```

## Verification

### On control plane, check nodes for presence of target version
```
k get nodes
```

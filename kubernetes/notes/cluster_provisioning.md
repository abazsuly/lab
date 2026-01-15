## Pre-Setup Notes

#### Resources
Vagrantfile and associated lab files found here: https://github.com/kodekloudhub/certified-kubernetes-administrator-course/tree/master/kubeadm-clusters/virtualbox

Official provisioning guide with kubeadm found here: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

Official CRI installation guide here: https://kubernetes.io/docs/setup/production-environment/container-runtimes/

Official add-on guide, commonly used to install CNI like flannel: https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy


## Setting up a 3-Node Cluster (no HA, containerd)

### Pre setup
1. Set up nodes from scratch using 3 node Vagrant file:
```shell
vagrant up
```

### On all nodes...

2. Download public signing key for the Kubernetes package repo on all nodes. Create `/etc/apt/keyrings` if it does not exist.
```shell
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.35/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

3. Add Kubernetes package repository on all nodes. Ensure that correct version is provided, this is command for v1.35:
```shell
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.35/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

4. Upgrade all nodes using
```shell
sudo apt-get update
sudo apt-get upgrade
```

5. Install packages needed to use the Kubernetes apt repo on all nodes:
```shell
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

6. Install Kubernetes packages and pin the version on all nodes, as well as the CRI containerd:
```shell
sudo apt-get install kubeadm kubelet kubectl containerd
sudo apt-mark hold kubelet kubeadm kubectl
```

7. Enable kubelet via systemd on all nodes:
```shell
sudo systemctl enable kubelet
```

8. Disable swap on all nodes:
```shell
sudo swapoff -a
```

9. On all nodes, load the br_netfilter module and make it persistent across reboots:
```shell
sudo modprobe br_netfilter
echo br_netfilter | sudo tee /etc/modules-load.d/k8s.conf
```

10.  On all nodes, Set required sysctls and apply immediately, then restart containerd:
```shell
cat <<'EOF' | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system
sudo systemctl restart containerd 
```

11. Configure `systemd` cgroup driver. 
	1. Create the directory `/etc/containerd`
	2. Generate the default config
	3. Edit `SystemdCGroup` set to true under `runc.options`
	4. Restart containerd service
```shell
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml

cat /etc/containerd/config.toml | egrep 'runtimes|SystemdCgroup'
      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            SystemdCgroup = false  ## <-- this needs to be changed to true
            
sudo vim /etc/containerd/config.toml       ## <-- make edits

sudo systemctl restart containerd
```

### On the control plane node...

12. On the controlplane node, Init the cluster using the appropriate `--pod-network-cidr` for the provided CNI. In this case, we will use the default flannel pod-network-cidr. Also, we need to pass in the IP that the node is available at, it might not automatically select the correct one. Pass it in via `--apiserver-advertise-address`:
```shell
ip addr  ## <-- get working ip using this command to advertise
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.40.69
```

13. Note down the `kubeadm join ...` command given by the init of the controlplane, this is how we add the worker nodes to the cluster.

14. Enable kubectl work for non-root user:
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

15. Install flannel on controlplane node:
```shell
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```

16. Verify all pods are running on the controlplane node:
```
kubectl get pods -A
```
### On the worker nodes...
17. Run the join commands given by the controlplane on the worker nodes using sudo
```shell
sudo kubeadm join 192.168.40.69:6443 --token q5ratc.38381twiak2rj2kj \
	--discovery-token-ca-cert-hash sha256:586a766b7e6009c341a997962bf4fdd6e98676c018dcababe27ca105ced16cb5
```

### Finished!
18. All nodes should be present on controlplane via `kubectl get nodes` and all pods should be running via `kubectl get pods -A`





## Setting up 3-Node Cluster (no HA, cri-dockerd)

Main differences between containerd and cri-dockerd:
1. kubeadm will not automatically find the cri-dockerd socket and will need to be pointed at it during `init` phase
2. Installing cri-dockerd via packages and not via apt-get?


?.  Init the cluster using the appropriate `--pod-network-cidr` for the provided CNI. In this case, we will use the default flannel pod-network-cidr. Since we are using cri-dockerd, the cri-dockerd socket must also be passed:
```
kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket=unix:///var/run/cri-dockerd.sock
```
# Static Pod notes

### Default location of static pod yamls
`ls /etc/kubernetes/manifest`

### If non default location is used, check kubelet configuration
`cat /var/lib/kubelet/config.yaml | grep -i manifest`
`systemctl cat kubelet | grep -i manifest`

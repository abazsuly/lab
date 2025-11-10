# Taint Commands

### Taint a node preventing scheduling to node
`kubectl taint nodes node-name key=value:NoSchedule`

### Taint a node trying but not guaranteeing preventing scheduling to node
`kubectl taint nodes node-name key=value:PreferNoSchedule`

### Taint a node preventing new intolerant nodes from being created and evicting intolerant existing intolerant nodes
`kubectl taint nodes node-name key=value:NoExecute`

### Remove a taint from a node (minus '-' at end of taint)
`kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-`
`kubectl taint nodes controlplane key=value:NoSchedule-`

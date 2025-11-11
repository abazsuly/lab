# Namespace Commands

### Get namespaces
`kubectl get ns`

### Create namespace imperatively
`kubectl create ns nameofnamespace`

### Switch namespace permanently
`kubectl config set-context $(kubectl config current-context) --namespace=my-namespace`

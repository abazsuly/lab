# Deployment Commands

### Create deployment
`kubectl create -f deployment-definition.yaml`

### Get deployments
`kubectl get deployments`

### Generate deployment yaml
`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`

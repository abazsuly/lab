# Deployment Commands

### Create deployment
`kubectl create -f deployment-definition.yaml`

### Get deployments
`kubectl get deployments`

### Generate deployment yaml
`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`

### Update image of deployment (note: this does not change a saved yaml definition)
`kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1`
`kubectl set image deploy appname containername=newimg1.1`

### Check status of deployment rollout
`kubectl rollout status deployment/myapp-deployment`

### Check history of deployment rollout
`kubectl rollout history deployment/myapp-deployment`

### Undo deployment rollout to previous version
`kubectl rollout undo deployment/myapp-deployment`

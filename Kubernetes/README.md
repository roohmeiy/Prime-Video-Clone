## Create Deployment and Service using Imperative method
### Deployment
```
kubectl create deployment prime-deploy --port=80 --dry-run=client -o yaml > prime-deploy.yaml
```
### Expose Service
```
kubectl expose prime-deploy --type=NodePort --port=80 --target-port=80 --name=prime-svc --dry-run=client -o yaml > prime-svc.yaml
```
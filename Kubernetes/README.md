## Create Deployment and Service using Imperative method
### Deployment
```
kubectl create deployment prime-deploy --port=80 --dry-run=client -o yaml > prime-deploy.yaml
```
### Expose Service
```
kubectl expose prime-deploy --type=NodePort --port=80 --target-port=80 --name=prime-svc --dry-run=client -o yaml > prime-svc.yaml
```

---
### Installation Instruction For EKS
- Install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
- Install Kubectl
```
sudo snap install kubectl --classic
kubectl version --client
```

- Install EKS CTL
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin

```

- Create EKS Cluster
```
eksctl create cluster \
  --name sen-devops \
  --region ap-south-1 \
  --nodegroup-name prime-1 \
  --nodes 1 \
  --node-type t3.medium \
  --managed

```
- Update kubeconfig to Access the EKS Cluster
```
aws eks --region ap-south-1 update-kubeconfig --name sen-devops
```
- Verify the Connection to the EKS Cluster
```
kubectl get nodes
```

-  Delete the EKS Cluster
```
eksctl delete cluster \
  --name sen-devops \
  --region ap-south-1
```
- Verify Deletion
```
eksctl get clusters --region ap-south-1
```
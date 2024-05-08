# Crossplane & ArgoCD Tutorial
I am using minikube for deploy k8s 
## Install ArgoCD
### Create a new namespace argocd and deploy ArgoCD 
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get all -n argocd
```

## Install Crossplane on Kubernetes

```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
helm search repo crossplane
helm install crossplane \
    --namespace crossplane-system \
    --create-namespace \
    --version 1.13.2 \
    crossplane-stable/crossplane
# Apply application crossplane in Argocd 
kubectl apply -f argocd.yaml

```
![alt text](image-4.png)


## Create AWS VPC using Crossplane

```bash
kubectl apply -f 0-crossplane/1-provider-aws-ec2.yaml
# Check until provider status "Healthy"
kubectl get providers
kubectl apply -f 1-vpc
kubectl get VPC
kubectl get InternetGateway
kubectl get RouteTableAssociation
```
![alt text](image.png)
![alt text](image-6.png)
![alt text](image-5.png)
## Create EKS Cluster using Crossplane
```bash
kubectl apply -f 0-crossplane
# Check until provider status "Healthy"
kubectl get providers
kubectl apply -f 2-eks
kubectl get Cluster
kubectl get NodeGroup
aws configure --profile crossplane
aws eks update-kubeconfig --name dev-demo --region us-east-2 --profile crossplane
kubectl get nodes
```
![alt text](image-7.png)

## Create OpenID Connect Provider (OIDC)

```bash
kubectl apply -f 3-irsa
kubectl get OpenIDConnectProvider
kubectl get Addon
```


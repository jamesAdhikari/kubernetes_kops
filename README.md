# Prerequisites:
- Ubuntu instance (You may use other linux instance as well)
- AWS-cli setup
- S3 bucket

# create aws hosted zone
```	
aws route53 create-hosted-zone --name kops.yourdomain.com
```
# create s3 storage 

```
aws s3 mb s3://storage.kops.yourdomain.com

```
# Install kubectl

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

```
# install kops

```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops

```
# Create Kubernetes Cluster
```
export KOPS_STATE_STORE=s3://storage.dev.yourdomain.com

kops create cluster --cloud=aws --dns-zone=kops.yourdomain.com --master-size=t2.micro --master-zones=ap-southeast-1a --node-count=2 --node-size=t2.micro --zones=ap-southeast-1a,ap-southeast-1b --name=kops.yourdomain.com

kops update cluster kops.yourdomain.com --yes
```
#
```
kubectl get nodes
```


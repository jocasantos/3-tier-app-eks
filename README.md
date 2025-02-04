# Deploy a E-Commerce 3-Tier-App on AWS EKS

### Pre-requisites
- AWS CLI
- kubectl
- eksctl

### Create a EKS cluster
```
eksctl create cluster --name demo-cluster-three-tier-1 --region eu-north-1
```

### Configure IAM OIDC provider 
```
export cluster_name=demo-cluster-three-tier-1
```
```
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5) 
```
- Check if there is an IAM OIDC provider configured already

```
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "/" -f4
```
- If not, run the below command
```
eksctl utils associate-iam-oidc-provider --cluster $cluster_name --approve
```

### Deploy ALB 
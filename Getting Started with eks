# eks-cluster-creation


##################### Create AWS EKS clsuster #######################################################################

## Pre-requisite links  - for updating awscli, kubectl and syncing kubectl cli to eks cluster.
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

## Create EKS cluster
eksctl create cluster --name eksrbac --node-type t2.medium --nodes 1 --nodes-min 1 --nodes-max 2 --region us-east-1 --zones=us-east-1a,us-east-1b,us-east-1c

## Get EKS Cluster service
eksctl get cluster --name eksrbac --region us-east-1

## Get EKS Pod data.
kubectl get pods --all-namespaces

## Delete EKS cluster
eksctl delete cluster --name eksrbac --region us-east-1


#################### EKS Cluster RBACs #############################################################################

## first create the rbac-test namespace, and then install nginx into it
kubectl create namespace rbac-test

## Deploy nginx pod on clsuster
kubectl create deploy nginx --image=nginx -n rbac-test

## To verify the test pods were properly installed
kubectl get all -n rbac-test

## Create IAM user and create access key - this can also be done through UI
aws iam create-user --user-name rbac-user
aws iam create-access-key --user-name rbac-user

## We will use these above creds to log in to the other user account.
aws configure.

## MAP AN IAM USER TO K8S
kubectl apply -f .\aws-auth.yaml

## Verify newly created user after login AND it should throw error because we have not yet created and binded the role to eks cluster.
kubectl get pods -n rbac-test

## Create a role and role binding from Admin access.
kubectl apply -f rbac-user-role.yaml
kubectl apply -f rbac-user-role-binding.yaml

## login with Kubernetes user creds again
## Verify newly created user after login AND it should Not throw any errors
kubectl get pods -n rbac-test

## Verify newly created user after login AND it should throw errors because we havnt provided rules to list resources in kube-system namespace.    #Just a verification step.
kubectl get pods -n kube-system

####################################################################################################################

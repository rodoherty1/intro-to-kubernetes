
# Getting Started with Amazon EKS

To compare the experience of deploying a simple application into Google Kubernetes Engine, we also deployed the same application into Amazon EKS (AWS ECS for Kubernetes).

To the uninitiated, the EKS experience is a bit more complicated than the GKE experience.


It is recommended that you first create a user that will administer the cluster.

## Creating IAM resources
    * Create an IAM Role for the EKS Cluster that we are about to create.  This role will be used by Kubernetes so that it can create AWS resources on our behalf.
    
    * Create a new IAM Policy called `my-eks-policy` with the following spec:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole",
                "eks:*",
                "ecr:*"
            ],
            "Resource": "*"
        }
    ]
}
```

    * Create IAM user called `k8sadmin`

## Creating the EKS Kubernetes Control Plane
    * aws eks create-cluster --name [MY_EKS_CLUSTER_NAME] --role-arn arn:aws:iam::111122223333:role/eks-service-role --resources-vpc-config subnetIds=[MY_SUBNET1],[MY_SUBNET2],securityGroupIds=[MY_SECURITY_GROUP]

    * aws eks describe-cluster --name [MY_EKS_CLUSTER_NAME] --query cluster.status

    * aws eks update-kubeconfig --name [MY_EKS_CLUSTER_NAME]

## Setting up `docker`
    * $(aws ecr get-login --region eu-west-1 --no-include-email)

    * docker tag [MY_DOCKER_IMAGE] [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[MY_APPLICATION_NAME]:v1

    * docker push [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[MY_APPLICATION_NAME]:v1

## Create EKS Worker nodes

## Useful Links

[AWS EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)

## TODO

* Mention that CloudFormation is required to set up a separate VPC for each K8s cluster.

* The template is supplied by the EKS service and costs $0.20 an hour to run.

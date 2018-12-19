
# Getting Started with Amazon EKS

* Create IAM user called `k8sadmin`
* Create a new Policy called `my-eks-policy` with the following spec:
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
* aws eks create-cluster --name [MY_EKS_CLUSTER_NAME] --role-arn arn:aws:iam::111122223333:role/eks-service-role --resources-vpc-config subnetIds=[MY_SUBNET1],[MY_SUBNET2],securityGroupIds=[MY_SECURITY_GROUP]

* aws eks describe-cluster --name [MY_EKS_CLUSTER_NAME] --query cluster.status

* aws eks update-kubeconfig --name [MY_EKS_CLUSTER_NAME]

* $(aws ecr get-login --region eu-west-1 --no-include-email)

* docker tag [MY_DOCKER_IMAGE] [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[MY_APPLICATION_NAME]:v1

* docker push [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[MY_APPLICATION_NAME]:v1


## TODO
Mention that CloudFormation is required to set up a separate VPC for each K8s cluster.
The template is supplied by the EKS service and costs $0.20 an hour to run.

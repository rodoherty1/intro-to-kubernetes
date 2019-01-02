
# Getting Started with Amazon EKS

To compare the experience of deploying a simple application into Google Kubernetes Engine, we also deployed the same application into Amazon EKS (AWS ECS for Kubernetes).

To the uninitiated, the EKS experience is a bit more complicated than the Google Kubernetes Engine experience.

It is recommended that you first create a user that will administer the Kubernetes cluster.  Details below!

This page is simply a summary of how I deployed our sample java application in AWS EKS.

Throughout this exercise, I was following Amazon's excellent [EKS Getting Started](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html) guide.

## Cost of this exercise

For this exercise, two Cloudformation templates will be launched.  One for the Kubernetes Control Plane and one for the Data Plane.

The cost of the Control Plane is determined by AWS itself is EUR €0.20 an hour to run.

The cost of the Data Plane is determined by your choice of hardware that will underpin your Kubernetes Worker Nodes.  I chose 3 * `T3.small` instances which are about EUR €0.02 an hour to run.


## Creating IAM resources

* Create an IAM Role for the EKS Cluster that we are about to create.  I named this role `eks-service-role`.  This role will be used by Kubernetes so that it can create AWS resources on our behalf.
    
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

I turned to the AWS Console for this part of the deployment.

A CloudFormation template must be completed which will describe the hardware that underpins our worker nodes.

To minimise costs, I attempted to launch `T3.micro` class instances but I noticed that some pods remaining at status `PENDING`.  

When I switched from `T3.micro` to `T3.small`, the pods successfully transitioned to state RUNNING..

## Final thoughts

Happily, I did not need to make any significant changes to the `yaml` files that describe my Kubernetes deployments, services or persistent-storage-claims.

The only necessary change was to name of the docker image of our application which is understandable because it is located in Amazon ECR rather than Google GKR.

I was especially pleased that the yaml for the persistent-storage-claim (for Cassandra) did not need any change.  AWS provisioned an EBS Volume and assigned it to the EC2 Instance that was running the Cassandra Pod.

Overall, the experience is not as simple as the Google GKE experience but the excellent documentation help a lot.

## Useful Links

[AWS EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)


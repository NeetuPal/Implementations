# Implementations
EKS Install with fargate and deploy app with ingress
====================================================
'''
Deploying an Application on EKS Cluster with AWS Fargate and ALB
Overview:
I deployed an application on an Amazon EKS cluster using AWS Fargate, where workloads run without managing EC2 nodes. The application was exposed via an AWS Application Load Balancer (ALB). Below is a structured approach to how I achieved this:

Prerequisites:
Before deployment, I ensured the required tools were installed:

AWS CLI – to interact with AWS services

kubectl – to manage Kubernetes clusters

eksctl – to create and configure EKS clusters

Step-by-Step Deployment:
Provisioning the EKS Cluster:

Used eksctl to create an EKS cluster with Fargate as the compute layer.

Defined the necessary VPC and networking settings.

Creating a Fargate Profile:

Configured a Fargate profile to allow workloads in a specific Kubernetes namespace to run on AWS Fargate.

This ensures that workloads are scheduled on Fargate without needing EC2 nodes.

Deploying Application Resources:

Applied Kubernetes YAML manifests (Deployments, Services, ConfigMaps, etc.) to the designated namespace using kubectl apply.

Ensured all resources were correctly scheduled on Fargate.

Associating OIDC Provider with EKS Cluster:

Enabled IAM OIDC provider for the EKS cluster to allow IAM roles for service accounts (IRSA).

This step is crucial for fine-grained IAM permission management.

Setting Up ALB Controller Permissions:

Created an IAM policy for the ALB Ingress Controller.

Attached the policy to an IAM role and associated it with a Kubernetes service account (SA).

Installing ALB Ingress Controller:

Added the Helm repository for AWS Load Balancer Controller.

Installed and configured the controller to integrate with the EKS cluster.

Accessing the Application:

Verified that the ALB was correctly provisioned.

Retrieved the ALB DNS name and accessed the deployed application over the internet.
'''

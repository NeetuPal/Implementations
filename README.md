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

DevOpsified a GoLang app hosted on GitHub by setting up a fully automated CI/CD pipeline with AWS EKS.
===================================================================
'''
Project Overview:
"I implemented a complete DevOps pipeline for a GoLang application, automating its deployment on AWS EKS using GitHub Actions, Helm, and ArgoCD. Here’s the step-by-step approach I followed:"

🔹 Step 1: Pre-requisites Setup
"I ensured all required tools were installed, including Git, Docker, AWS CLI, eksctl, kubectl, Helm, and ArgoCD."

🔹 Step 2: EKS Cluster Deployment
"I provisioned an EKS cluster along with a node pool using eksctl to host the application."

🔹 Step 3: Application Testing & Containerization
"I first tested the GoLang application locally. Then, I containerized it using Docker and tested it again inside a container."

🔹 Step 4: Kubernetes Manifests for Deployment
"I wrote Kubernetes manifests for Deployment, Service, and Ingress to define how the application should be deployed, exposed, and accessed."

🔹 Step 5: Deploying Kubernetes Manifests Manually
"I applied the Kubernetes manifests step by step using kubectl, without Helm, to validate that the application worked as expected."

🔹 Step 6: Testing via NodePort
"I exposed the application using NodePort and accessed it via the worker node’s IP to ensure it's reachable."

🔹 Step 7: Ingress Controller Setup
"To enable external access, I set up an Ingress NGINX Controller and validated application access via LoadBalancer and Network Load Balancer (NLB)."

🔹 Step 8: Helm Chart Creation
"To simplify deployments, I converted the raw Kubernetes manifests into a Helm chart and packaged the application for better reusability and parameterization."

🔹 Step 9: Deploying with Helm
"I used Helm to deploy all Kubernetes resources in a single command, making deployment easier and configurable."

sh
Copy
Edit
helm install go-web-app ./go-web-app-chart
🔹 Step 10: Implementing CI with GitHub Actions
"I automated the CI pipeline using GitHub Actions. The workflow included:"
✅ Build & Push Docker Image to Amazon ECR
✅ Update the Helm values.yaml with RUN_ID as the new image tag
✅ Push changes back to GitHub
✅ Store AWS credentials & other secrets in GitHub repository secrets

🔹 Step 11: ArgoCD Installation & Setup
"For automated continuous deployment, I installed ArgoCD and patched the secrets to configure access. I retrieved the login password by decoding ArgoCD’s secret."

sh
Copy
Edit
kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode
🔹 Step 12: Deploying with ArgoCD & Validating GitOps Workflow
"I created an application in ArgoCD to sync my Helm chart-based deployment with the Git repository. This ensured that any change pushed to GitHub would be automatically deployed via ArgoCD."

🔹 Step 13: End-to-End CI/CD Validation
"Finally, I tested the complete CI/CD pipeline by pushing a code change to GitHub and verifying that:"
✅ GitHub Actions builds & pushes the Docker image
✅ ArgoCD detects the update & deploys the new version
✅ The application is live with the latest changes

🚀 Key Takeaways & Impact
Automated deployments with GitOps (Helm + ArgoCD)

CI/CD pipeline in GitHub Actions for seamless delivery

Scalable, reproducible, and fully automated infrastructure

🎤 Example Answer in an Interview:
"I DevOpsified a GoLang app hosted on GitHub by setting up a fully automated CI/CD pipeline with AWS EKS. First, I created the EKS cluster and tested the app locally before containerizing it. I deployed Kubernetes manifests manually and then optimized the deployment using Helm. Next, I integrated CI/CD using GitHub Actions to build, push, and update the image tag dynamically. Finally, I set up ArgoCD for GitOps-based automated deployments. This approach streamlined the deployment process, ensuring high availability, scalability, and repeatability."
'''

EKS and VPC Module Implementation in Terraform 
===================================================================================================
'''
Explaining EKS and VPC Module Implementation in Terraform to an Interviewer
When explaining your EKS and VPC module implementation using Terraform, focus on:
✅ The high-level architecture (what you're building).
✅ The key components (how they're structured).
✅ Why specific configurations were used (best practices).

Step-by-Step Explanation:
1️⃣ Pre-requisites
Before deploying EKS, we need the following installed on our local machine:

Git (to clone the repository).

AWS CLI (to interact with AWS services).

Terraform (to provision infrastructure).

2️⃣ Clone Terraform Code
We start by cloning the Terraform repository from a version-controlled system like GitHub or Bitbucket:

sh
Copy
Edit
git clone <repo-url>
cd terraform-eks-project
This ensures we're working with the latest infrastructure as code (IaC).

3️⃣ VPC Module Implementation
We define a VPC module to create a secure networking layer:

Public subnets for external-facing ELB (Application Load Balancer).

Private subnets for internal EKS nodes and Internal Load Balancer (for security and cost efficiency).

Example Terraform VPC module:

hcl
Copy
Edit
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.14"

  name = "eks-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-east-1a", "us-east-1b"]
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnets = ["10.0.3.0/24", "10.0.4.0/24"]

  enable_nat_gateway = true
  enable_dns_hostnames = true
}
Why?
✔️ Public subnets for ELB allow internet access to applications.
✔️ Private subnets for EKS ensure worker nodes are secure and not publicly exposed.
✔️ NAT Gateway allows private subnets to access the internet without exposing instances.

4️⃣ Security Group for EKS Managed Nodes
We create Security Groups to define access control for EKS worker nodes:

Allows inbound traffic from EKS control plane.

Allows outbound traffic to AWS services like S3.

Example Security Group:

hcl
Copy
Edit
resource "aws_security_group" "eks_nodes" {
  name_prefix = "eks-nodes-sg"
  vpc_id      = module.vpc.vpc_id

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
Why?
✔️ Allows EKS worker nodes to communicate with the control plane securely.
✔️ Restricts unnecessary access, following least privilege principle.

5️⃣ Creating EKS Managed Node Group
We define an EKS Managed Node Group to manage worker nodes efficiently:

Min, max, and desired instances ensure scalability.

Uses custom AMI and EC2 instance types for performance optimization.

Example:

hcl
Copy
Edit
module "eks_node_group" {
  source           = "terraform-aws-modules/eks/aws//modules/node_groups"
  cluster_name     = module.eks.cluster_id
  node_group_name  = "managed-ng"
  
  node_groups = {
    managed_nodes = {
      desired_capacity = 2
      max_capacity     = 5
      min_capacity     = 1

      instance_types = ["t3.medium"]
      ami_type       = "AL2_x86_64"
    }
  }
}
Why?
✔️ Autoscaling ensures high availability.
✔️ Uses Amazon Linux 2 AMI for optimized performance.
✔️ t3.medium instances balance cost and performance.

6️⃣ Deploying EKS in Private Subnets
We create an EKS module and deploy it into private subnets for security:

Uses a specific Kubernetes version for compatibility.

Private subnets keep worker nodes protected from direct internet access.

Example:

hcl
Copy
Edit
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "18.0"

  cluster_name    = "abhi-eks-cluster"
  cluster_version = "1.27"

  subnets         = module.vpc.private_subnets
  vpc_id          = module.vpc.vpc_id
}
Why?
✔️ Private subnets protect worker nodes from public exposure.
✔️ Latest Kubernetes version ensures security and feature compatibility.

Summarizing to the Interviewer
💡 "In our Terraform setup, we implemented an EKS cluster with a custom VPC module. The VPC has public subnets for ELB and private subnets for worker nodes. Security Groups restrict access to nodes, and a Managed Node Group handles scaling. Finally, we deployed EKS in private subnets using a specific Kubernetes version for security and reliability."


'''
Deploying an AKS Cluster Using Terraform Custom Modules
===================================================================
'''
1️⃣ High-Level Overview
"I deployed an Azure Kubernetes Service (AKS) cluster using Terraform custom modules to ensure modularity, reusability, and maintainability. The deployment was structured as follows:"

✅ Tools Used:

Azure CLI – For authentication and resource management.

Git – Version control for Terraform code.

Terraform – Infrastructure as Code (IaC) to provision resources.

VS Code – Used as the development environment.

2️⃣ Step-by-Step Explanation
Step 1: Pre-requisites
"I ensured that the necessary tools (az cli, terraform, git) were installed and authenticated with Azure using az login."

Step 2: Created a Service Principal (SP) with RBAC
Since Terraform needs authentication to manage Azure resources, I created a Service Principal (SP) with Contributor role:

sh
Copy
Edit
az ad sp create-for-rbac --name "terraform-sp" --role="Contributor" --scopes="/subscriptions/<SUBSCRIPTION_ID>"
Exported SP credentials as environment variables for Terraform:

sh
Copy
Edit
export ARM_CLIENT_ID="xxxxxx"
export ARM_CLIENT_SECRET="yyyyyy"
export ARM_SUBSCRIPTION_ID="zzzzzz"
export ARM_TENANT_ID="aaaaaa"
Used these credentials in Terraform’s Azure provider configuration.

Step 3: Created a Resource Group & Storage Account for Terraform State
Since Terraform needs to store state files, I created:

An Azure Resource Group.

A Storage Account.

A Blob Storage Container to store Terraform state.

Terraform configuration for backend storage:

hcl
Copy
Edit
terraform {
  backend "azurerm" {
    resource_group_name   = "my-rg"
    storage_account_name  = "mytfstate"
    container_name        = "tfstate-container"
    key                   = "terraform.tfstate"
  }
}
Step 4: Created Terraform Custom Modules
Modularized Infrastructure:

SP Module: Created an Azure AD Service Principal.

AKS Module: Provisioned the AKS cluster.

Key Vault Module: Created a Key Vault to store sensitive credentials.

📌 Example of Terraform Custom Module Structure:

css
Copy
Edit
terraform-aks/
│── main.tf
│── variables.tf
│── outputs.tf
│── modules/
│   ├── aks/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   ├── keyvault/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   ├── service-principal/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
Step 5: Provisioned AKS Cluster Using Terraform
Created an AKS cluster inside a private subnet.

Defined node pools with min/max count for autoscaling.

Enabled RBAC & Azure AD integration.

📌 Example of Terraform Code for AKS Module (modules/aks/main.tf)

h
Copy
Edit
resource "azurerm_kubernetes_cluster" "aks" {
  name                = var.cluster_name
  location            = var.location
  resource_group_name = var.resource_group_name
  dns_prefix          = var.dns_prefix

  default_node_pool {
    name       = "default"
    node_count = var.node_count
    vm_size    = var.vm_size
  }

  identity {
    type = "SystemAssigned"
  }
}
📌 Using the AKS Module in main.tf

hcl
Copy
Edit
module "aks" {
  source              = "./modules/aks"
  cluster_name        = "my-aks-cluster"
  location            = "East US"
  resource_group_name = "my-rg"
  node_count          = 3
  vm_size             = "Standard_DS2_v2"
}
Step 6: Stored Secrets in Azure Key Vault
After deploying the Service Principal, its credentials were stored in Azure Key Vault:

hcl
Copy
Edit
resource "azurerm_key_vault_secret" "client_secret" {
  name         = "aks-client-secret"
  value        = module.sp.client_secret
  key_vault_id = module.keyvault.id
}
Step 7: Applied Terraform Configuration
Ran Terraform Commands to Deploy Infrastructure:

sh
Copy
Edit
terraform init
terraform plan
terraform apply -auto-approve
Step 8: Retrieved Kubeconfig and Connected to AKS
Downloaded kubeconfig for connecting to AKS:

sh
Copy
Edit
az aks get-credentials --resource-group my-rg --name my-aks-cluster
Verified AKS Cluster:

sh
Copy
Edit
kubectl get nodes
3️⃣ Summary (for the Interviewer)
"I deployed an Azure Kubernetes Service (AKS) cluster using Terraform custom modules for modularity and maintainability. My approach included:

✔ Pre-requisites: Installed az cli, terraform, git, and vs code.
✔ Authentication: Created a Service Principal with RBAC permissions.
✔ State Management: Stored Terraform state in an Azure Storage Account.
✔ Custom Modules: Used separate Terraform modules for AKS, Key Vault, and Service Principal.
✔ Secure Secrets Storage: Stored sensitive credentials in Azure Key Vault.
✔ Deployment: Used terraform apply to provision AKS with autoscaling & RBAC.
✔ Validation: Retrieved kubeconfig and verified the cluster using kubectl.

This modular approach improved scalability, security, and reusability across projects."

🎯 Follow-up Questions an Interviewer Might Ask
How did you handle role-based access control (RBAC) for AKS?
→ Enabled RBAC in AKS, used Azure AD integration, and assigned least privilege roles.

How do you ensure Terraform state security?
→ Stored Terraform state in an Azure Storage Account with RBAC & encryption.

How do you handle secrets in Terraform?
→ Used Azure Key Vault and prevented hardcoding secrets in Terraform files.

How do you implement autoscaling in AKS?
→ Configured node pools with min/max count and enabled Horizontal Pod Autoscaler (HPA).


'''

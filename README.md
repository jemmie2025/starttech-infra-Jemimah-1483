# StartTech Infrastructure Repository

This repository contains the Infrastructure as Code (IaC) and deployment automation for the StartTech application. It provisions a scalable, highly available, and production-ready AWS infrastructure using Terraform, with integrated monitoring, security, and CI/CD readiness.

## Overview

The StartTech Infrastructure repository automates the provisioning and management of AWS resources using Terraform. It follows industry best practices for high availability, scalability, observability, and security, enabling reliable and repeatable infrastructure deployments.

### Key Capabilities

- Modular and reusable Terraform architecture
- Multi-AZ deployment for fault tolerance
- Auto Scaling based on workload demand
- Application Load Balancer with health checks
- Container image management using Amazon ECR
- Managed database, caching, and storage services
- Centralized logging and monitoring with CloudWatch
- CI/CD-ready for GitHub Actions
- Zero-downtime infrastructure updates

## Architecture

```
CloudFront (CDN)
│
├── S3 (Static Assets)
│
Application Load Balancer (ALB)
│
Auto Scaling Group (EC2 Instances)
│
├── Database (Amazon DocumentDB / RDS)
├── Cache (ElastiCache – Redis)
└── Object Storage (S3)
│
CloudWatch (Logs, Metrics, Dashboards, Alarms)
```

CloudFront accelerates static content delivery, while dynamic traffic flows through the Application Load Balancer. Compute capacity is managed by Auto Scaling Groups, and system health is monitored centrally using CloudWatch.

## AWS Services

- VPC – Network isolation and routing
- EC2 – Application compute
- ALB – Load balancing and health checks
- Auto Scaling – Elastic capacity management
- ECR – Container image registry
- S3 – Static assets and backups
- CloudFront – Content delivery and caching
- DocumentDB / RDS – Managed database
- ElastiCache (Redis) – In-memory caching
- CloudWatch – Monitoring, logs, alarms
- IAM – Access control and permissions

## Prerequisites

### Required tools:

- Terraform version 1.0 or higher
- AWS CLI version 2
- Git
- Bash (Linux/macOS) or PowerShell (Windows)

### AWS requirements:

- An active AWS account
- IAM credentials with sufficient permissions
- AWS region configured (default is us-east-1)

### Optional tools:

- Docker
- Terraform remote state backends
- AWS SSO

## Getting Started

### Configure AWS Credentials

Run: `aws configure`

Or authenticate with SSO using: `aws sso login --profile your-profile`

### Initialize Terraform

Navigate to the Terraform directory and run:

```bash
cd terraform
terraform init
```

### Review Infrastructure Plan

Generate and review the plan using:

```bash
terraform plan -out=tfplan
```

### Apply Infrastructure

Deploy the infrastructure with:

```bash
terraform apply tfplan
```

### Verify Deployment

View outputs using: `terraform output`

Check running instances using: `aws ec2 describe-instances`

Check load balancers using: `aws elbv2 describe-load-balancers`

## Repository Structure

```
starttech-infra
├── .github/workflows          # CI/CD pipelines
├── terraform                  # Root Terraform configuration
│   ├── main.tf               # Entry point
│   ├── variables.tf          # Input variables
│   ├── outputs.tf            # Output values
│   ├── terraform.tfvars.example  # Sample variables
│   └── modules/
│       ├── networking        # VPC and networking
│       ├── compute           # EC2, ALB, Auto Scaling
│       ├── storage           # S3, ECR, databases
│       └── monitoring        # CloudWatch resources
├── scripts                   # Deployment automation
├── monitoring                # Dashboards and alarm definitions
├── README.md
└── .gitignore
```

## Terraform Modules

### Networking module

Provisions VPCs, subnets, routing, and security groups.

### Compute module

Manages EC2 instances, Application Load Balancer, and Auto Scaling Groups.

### Storage module

Provisions S3 buckets, ECR repositories, and database services.

### Monitoring module

Configures CloudWatch log groups, dashboards, alarms, and notifications.

Each module is isolated, reusable, and configurable.

## Deployment

### Automated Deployment

Infrastructure deployment is automated using GitHub Actions and triggered by:

- Pushes to the main branch
- Manual workflow execution
- Versioned releases

### Manual Deployment

Run `terraform plan -var-file=terraform.tfvars`

Then apply with `terraform apply -var-file=terraform.tfvars`

### Destroy Infrastructure

Tear down resources using: `terraform destroy -var-file=terraform.tfvars`

## Monitoring and Observability

CloudWatch provides centralized visibility into:

- EC2 CPU and network metrics
- ALB request count and latency
- Cache performance and evictions
- Application and infrastructure logs

Alarms notify via SNS for high CPU usage, unhealthy targets, latency spikes, cache evictions, and elevated error rates.

## Troubleshooting

### Terraform

- Validate configuration using: `terraform validate`
- Check formatting using: `terraform fmt -check -recursive`
- Inspect state using: `terraform state list`

### AWS Access

- Verify identity using: `aws sts get-caller-identity`
- List instances using: `aws ec2 describe-instances`

### Monitoring

- List log groups using: `aws logs describe-log-groups`
- View alarms using: `aws cloudwatch describe-alarms`

## Configuration Variables

Create a variables file by running:

```bash
cp terraform/terraform.tfvars.example terraform/terraform.tfvars
```

Edit values such as region, instance type, Auto Scaling limits, log retention days, and alert email addresses.

## Security Best Practices

- Terraform state is stored remotely and encrypted
- IAM roles follow least-privilege access principles
- Credentials are never committed to source control
- Encryption is enabled at rest and in transit
- Centralized logging and auditing are enforced

## Support

If issues occur:

- Review Terraform output and logs
- Inspect CloudWatch dashboards and alarms
- Verify AWS credentials and region
- Open an issue in the repository

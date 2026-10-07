<h1 align="center">☁️ AWS Multi-Tier Infrastructure with Terraform</h1>

<p align="center">
  Production-grade, secure, and scalable AWS infrastructure provisioned using reusable Terraform modules.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=FF9900"/>
  <img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white"/>
  <img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge"/>
</p>

---

## 📌 Overview

This project provisions a highly available, three-tier web architecture on AWS using Infrastructure as Code (IaC). It follows AWS Well-Architected best practices for security, reliability, and cost optimization.

**Problem solved:** Manual infrastructure setup is slow, inconsistent, and error-prone.
**Solution:** Fully automated, repeatable, and version-controlled deployments with Terraform.

---

## 🏗️ Architecture

```mermaid
flowchart TB
    U[Users] --> R53[Route 53]
    R53 --> ALB[Application Load Balancer]
    subgraph VPC["VPC 10.0.0.0/16"]
        subgraph Public["Public Subnets (2 AZs)"]
            ALB
            NAT[NAT Gateway]
        end
        subgraph Private["Private Subnets (2 AZs)"]
            ASG[EC2 Auto Scaling Group]
        end
        subgraph DB["Database Subnets (2 AZs)"]
            RDS[(RDS Multi-AZ)]
        end
    end
    ALB --> ASG
    ASG --> RDS
    ASG --> NAT
    ASG -.-> CW[CloudWatch]
```

> 📷 Add an exported diagram image here as well (draw.io or Lucidchart): `docs/architecture.png`

---

## ✨ Features

- ✅ Custom VPC with public, private, and database subnets across 2 Availability Zones
- ✅ Application Load Balancer with health checks
- ✅ EC2 Auto Scaling Group with scaling policies
- ✅ RDS (Multi-AZ) in isolated private subnets
- ✅ Remote state in S3 with DynamoDB state locking
- ✅ Least-privilege IAM roles and security groups
- ✅ CloudWatch alarms and logging
- ✅ CI/CD with GitHub Actions (`fmt`, `validate`, `tfsec`, `plan`)

---

## 🧰 Tech Stack

| Category | Tools |
|----------|-------|
| Cloud | AWS (VPC, EC2, ALB, ASG, RDS, S3, IAM, CloudWatch) |
| IaC | Terraform |
| CI/CD | GitHub Actions |
| Security | tfsec, Checkov, IAM, Security Groups |
| Monitoring | CloudWatch |

---

## 📁 Project Structure

```
.
├── modules/
│   ├── vpc/
│   ├── alb/
│   ├── asg/
│   └── rds/
├── environments/
│   ├── dev/
│   └── prod/
├── docs/
│   └── architecture.png
├── .github/
│   └── workflows/
│       └── terraform.yml
├── .gitignore
└── README.md
```

---

## ⚙️ Prerequisites

- AWS account with programmatic access
- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.5
- [AWS CLI](https://aws.amazon.com/cli/) configured (`aws configure`)
- S3 bucket and DynamoDB table for remote state

---

## 🚀 Deployment

```bash
# 1. Clone the repository
git clone https://github.com/<username>/<repo-name>.git
cd <repo-name>/environments/dev

# 2. Initialize Terraform
terraform init

# 3. Validate and preview changes
terraform validate
terraform plan -out=tfplan

# 4. Apply
terraform apply tfplan
```

**Output:** After a successful apply, Terraform prints the ALB DNS name. Open it in your browser to test.

---

## 🔐 Security Practices

- No credentials or secrets committed (`.gitignore` excludes `*.tfstate`, `*.tfvars`)
- Secrets stored in AWS Secrets Manager
- Encrypted S3 state bucket and RDS storage
- Private subnets for application and database tiers
- Security groups follow least privilege
- Automated scanning with tfsec in the CI pipeline

---

## 💰 Cost Estimate

| Resource | Approx. Monthly Cost (USD) |
|----------|----------------------------|
| NAT Gateway | ~$32 |
| ALB | ~$18 |
| EC2 (2 x t3.micro) | ~$15 |
| RDS (db.t3.micro, Multi-AZ) | ~$30 |

> ⚠️ Estimates only. Check the AWS Pricing Calculator for current prices. Destroy resources after testing.

---

## 🧹 Cleanup

```bash
terraform destroy
```

---

## 📸 Screenshots

| Deployed Application | CloudWatch Dashboard |
|---|---|
| ![app](docs/app.png) | ![cw](docs/cloudwatch.png) |

---

## 📚 What I Learned

- Designing reusable, modular Terraform code
- Managing remote state and locking safely
- Building secure multi-AZ network architecture
- Integrating security scanning into CI/CD pipelines

---

## 🔮 Future Improvements

- [ ] Add HTTPS with ACM certificate
- [ ] Add WAF in front of the ALB
- [ ] Migrate to EKS with ArgoCD
- [ ] Add cost monitoring with AWS Budgets

---

## 👩‍💻 Author

**Shilpa Khamaru**
AWS Cloud & DevOps Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/<your-username>)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/<your-username>)

---

⭐ If you found this project useful, please give it a star!

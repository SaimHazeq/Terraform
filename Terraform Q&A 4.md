# Terraform Interview Questions & Answers

## 📖 Overview

Terraform is an open-source Infrastructure as Code (IaC) tool developed by :contentReference[oaicite:0]{index=0} that enables provisioning, managing, and versioning cloud and on-premises infrastructure using declarative configuration files. Terraform uses providers to interact with cloud platforms and maintains infrastructure state through a state file.

---

# 🚀 What is Terraform?

Terraform allows you to define infrastructure using code and automate the creation, modification, and deletion of resources across cloud providers such as AWS, Azure, and GCP.

### Benefits

- Infrastructure as Code (IaC)
- Multi-cloud support
- Version-controlled infrastructure
- Automated provisioning
- Consistent deployments
- Dependency management

---

# 🔹 1. What is a State File in Terraform?

A state file (`terraform.tfstate`) is used by Terraform to track the current state of infrastructure and map resources defined in code to real-world resources.

### Why State Files Matter

- Tracks infrastructure changes
- Detects configuration drift
- Supports execution planning
- Enables resource dependencies

### View State

```bash
terraform show
```

---

# 🔹 2. How Can You Secure the State File?

Store state remotely with encryption and access control.

### Example: AWS S3 Backend

```hcl
terraform {
  backend "s3" {
    bucket  = "my-terraform-state"
    key     = "infra/terraform.tfstate"
    region  = "us-west-2"
    encrypt = true
  }
}
```

### Best Practices

- Enable encryption
- Restrict IAM access
- Enable versioning
- Use state locking

---

# 🔹 3. How Do You Manage Multiple Environments?

Terraform supports environment isolation through workspaces and variable files.

### Create Workspaces

```bash
terraform workspace new dev
terraform workspace new uat
terraform workspace new prod
```

### List Workspaces

```bash
terraform workspace list
```

### Switch Workspace

```bash
terraform workspace select prod
```

---

# 🔹 4. How Do You Import Existing Resources?

Use the `terraform import` command to bring existing infrastructure under Terraform management.

### Example

```bash
terraform import aws_instance.example i-1234567890abcdef0
```

---

# 🔹 5. How Do You Handle Secrets in Terraform?

Secrets should never be hardcoded.

### Recommended Methods

- AWS Secrets Manager
- Azure Key Vault
- HashiCorp Vault
- Environment Variables
- Sensitive Variables

### Example

```hcl
resource "aws_secretsmanager_secret" "db_secret" {
  name = "database-secret"
}

resource "aws_secretsmanager_secret_version" "db_secret_value" {
  secret_id = aws_secretsmanager_secret.db_secret.id

  secret_string = jsonencode({
    username = "admin"
    password = "securepassword"
  })
}
```

---

# 🔹 6. What is a Backend in Terraform?

A backend determines where Terraform stores its state.

### Backend Types

| Backend | Purpose |
|----------|----------|
| Local | Default state storage |
| S3 | AWS Remote State |
| Azure Blob | Azure Remote State |
| GCS | Google Cloud Storage |
| Terraform Cloud | Managed State Storage |

### Example

```hcl
terraform {
  backend "s3" {}
}
```

---

# 🔹 7. Difference Between `count` and `for_each`

## count

Creates multiple identical resources.

```hcl
resource "aws_instance" "server" {
  count         = 3
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

## for_each

Creates resources using unique keys.

```hcl
resource "aws_instance" "server" {
  for_each = toset(["app1", "app2"])

  ami           = "ami-123456"
  instance_type = "t2.micro"

  tags = {
    Name = each.key
  }
}
```

### Comparison

| count | for_each |
|---------|----------|
| Uses index | Uses key/value |
| Best for identical resources | Best for unique resources |

---

# 🔹 8. What Are Locals in Terraform?

Locals allow reusable values within a module.

### Example

```hcl
locals {
  instance_type = "t2.micro"
  ami_id        = "ami-123456"
}

resource "aws_instance" "web" {
  ami           = local.ami_id
  instance_type = local.instance_type
}
```

### Benefits

- Reduces duplication
- Improves readability
- Simplifies maintenance

---

# 🔹 9. What is Terraform Taint?

`terraform taint` marks a resource for recreation during the next apply.

### Example

```bash
terraform taint aws_instance.web
```

### Use Cases

- Resource corruption
- Manual infrastructure changes
- Forced replacement

---

# 🔹 10. What is a Null Resource?

A `null_resource` does not create infrastructure but allows execution of scripts and commands.

### Example

```hcl
resource "null_resource" "post_provision" {

  provisioner "local-exec" {
    command = "echo Infrastructure Created"
  }
}
```

### Common Use Cases

- Post-deployment scripts
- Automation workflows
- Trigger-based actions

---

# 🔹 Triggers in Null Resource

Triggers force re-execution when values change.

```hcl
resource "null_resource" "always_run" {

  triggers = {
    timestamp = timestamp()
  }

  provisioner "local-exec" {
    command = "echo Running"
  }
}
```

---

# 🔹 11. What is a Data Block?

Data blocks fetch existing resources without creating them.

### Example: Existing AMI

```hcl
data "aws_ami" "ubuntu" {

  most_recent = true

  owners = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/*"]
  }
}
```

Use the AMI:

```hcl
resource "aws_instance" "web" {

  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
}
```

---

# 🔹 Fetch Secret Using Data Source

```hcl
data "aws_secretsmanager_secret_version" "db_password" {
  secret_id = "prod/db_password"
}

output "password" {
  value = data.aws_secretsmanager_secret_version.db_password.secret_string
}
```

---

# 🔹 Read External File

```hcl
data "template_file" "script" {
  template = file("${path.module}/init.sh")
}

resource "aws_instance" "web" {
  user_data = data.template_file.script.rendered
}
```

---

# 🔹 What Happens During `terraform init`?

`terraform init` initializes the working directory.

### Background Activities

1. Downloads provider plugins
2. Initializes backend
3. Downloads modules
4. Validates configuration
5. Creates `.terraform` directory

### Example

```bash
terraform init
```

### Upgrade Providers

```bash
terraform init -upgrade
```

---

# 🔹 What Happens During `terraform plan`?

Terraform compares:

```text
Desired State (.tf)
        VS
Current State (tfstate)
        VS
Actual Infrastructure
```

### Operations Performed

1. Loads configuration
2. Loads providers
3. Reads state file
4. Queries cloud provider
5. Detects drift
6. Generates execution plan

### Example

```bash
terraform plan
```

Save Plan

```bash
terraform plan -out=tfplan
```

Apply Saved Plan

```bash
terraform apply tfplan
```

---

# 🔹 What is Terraform Lockfile?

Terraform generates:

```text
.terraform.lock.hcl
```

This file records exact provider versions and checksums.

### Benefits

- Consistent deployments
- Reproducible builds
- Prevents unexpected upgrades
- Secure plugin verification

### Example

```hcl
provider "registry.terraform.io/hashicorp/aws" {
  version = "5.0.0"
}
```

---

# 🔹 Update Lockfile

Refresh provider versions:

```bash
terraform init -upgrade
```

Delete and recreate:

```bash
rm .terraform.lock.hcl
terraform init
```

---

# 🖥️ Sample Terraform Code: Create EC2 Instance

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-123456789"
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-EC2"
  }
}
```

---

# 🌐 Sample Terraform Code: Create VPC and Subnet

```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "public" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
}
```

---

# 📦 Sample Terraform Code: Create S3 Bucket with Versioning

```hcl
resource "aws_s3_bucket" "bucket" {
  bucket = "terraform-demo-bucket"
}

resource "aws_s3_bucket_versioning" "versioning" {

  bucket = aws_s3_bucket.bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

---

# ⚡ Terraform Basic Commands

## Initialize Project

```bash
terraform init
```

## Validate Configuration

```bash
terraform validate
```

## Create Execution Plan

```bash
terraform plan
```

## Apply Changes

```bash
terraform apply
```

## Destroy Infrastructure

```bash
terraform destroy
```

---

# 🎯 Frequently Asked Terraform Interview Questions

### What is Terraform State?

Tracks real infrastructure and maps it to Terraform configuration.

### What is a Backend?

Stores and manages Terraform state.

### What is a Provider?

Plugin that allows Terraform to interact with cloud platforms.

### What is a Module?

Reusable collection of Terraform resources.

### What is a Data Source?

Reads existing infrastructure without creating resources.

### What is Drift?

Difference between actual infrastructure and Terraform state.

### What is a Workspace?

Environment isolation mechanism in Terraform.

### Difference Between Plan and Apply?

| Plan | Apply |
|--------|--------|
| Preview Changes | Execute Changes |
| No Infrastructure Changes | Creates/Updates Resources |

---

# 🏆 Key DevOps Skills Covered

`Terraform` `Infrastructure as Code` `AWS` `State Management` `Remote Backend` `S3 Backend` `Secrets Management` `Workspaces` `Modules` `Providers` `Terraform Cloud` `DevOps` `Cloud Automation` `IaC`

🚀 Terraform Complete Notes (Production-Ready)
📌 What is Terraform?

Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp that allows you to provision and manage infrastructure using code.

Language: HCL (HashiCorp Configuration Language)
Written in: Go
Released: 2014
Platform: Multi-cloud (AWS, Azure, GCP, On-prem)
🏗️ What is Infrastructure?

Infrastructure = Resources used to run applications.

Examples:

EC2 (Compute)
S3 (Storage)
VPC (Networking)
ELB (Load Balancer)
ASG (Auto Scaling)
❌ Problems with Manual Infrastructure
Time-consuming
Error-prone
Not scalable
No version control
✅ Why Terraform?
Infrastructure automation
Reusable code
Consistency
Supports dry-run (plan)
Multi-cloud support
⚙️ Terraform Workflow
terraform init
terraform plan
terraform apply
terraform destroy
Steps:
Init → Initialize providers/plugins
Plan → Preview execution
Apply → Create infrastructure
Destroy → Delete infrastructure
📁 Terraform Configuration
Example:
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}
🧠 Core Components
Provider → Cloud platform (AWS, Azure, etc.)
Resource → Infrastructure (EC2, S3, etc.)
Variables → Dynamic inputs
State File → Tracks infrastructure
Backend → Stores state (S3, etc.)
Modules → Reusable code
📦 Terraform State
File: terraform.tfstate
Stores real infrastructure details
Tracks changes

⚠️ If lost → Terraform cannot manage infrastructure

🔧 Terraform Commands
terraform init
terraform plan
terraform apply
terraform destroy
terraform fmt
terraform validate
terraform state list
🔢 Variables
variable.tf
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
Usage
instance_type = var.instance_type
📄 tfvars (Environment Config)
dev.tfvars
instance_type  = "t2.micro"
instance_count = 1
Run:
terraform apply -var-file="dev.tfvars"
🌍 Environment Variables
export TF_VAR_ami=ami-12345678
📤 Outputs
output "instance_ip" {
  value = aws_instance.example.public_ip
}
♻️ Terraform Lifecycle
Prevent Destroy
lifecycle {
  prevent_destroy = true
}
Create Before Destroy
lifecycle {
  create_before_destroy = true
}
Ignore Changes
lifecycle {
  ignore_changes = all
}
🔁 Count
resource "aws_instance" "example" {
  count = 3
}
🔄 For Each
for_each = toset(["dev", "test", "prod"])
🔗 Depends On
depends_on = [aws_s3_bucket.example]
🧩 Modules

Reusable Terraform code.

Example:
module "ec2" {
  source = "./modules/ec2"
}
🏢 Workspaces

Used for multiple environments.

terraform workspace list
terraform workspace new dev
terraform workspace select dev
terraform workspace delete dev
☁️ Backend (S3)
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
🧪 Provisioners
Local Exec
provisioner "local-exec" {
  command = "echo Hello"
}
Remote Exec
provisioner "remote-exec" {
  inline = [
    "sudo yum update -y"
  ]
}
🧱 Locals
locals {
  env = "dev"
}
🗂️ Dynamic Blocks

Used for looping blocks.

🧠 Terraform Taint
terraform taint aws_instance.example
terraform untaint aws_instance.example
🔄 Replace Resource
terraform apply -replace="aws_instance.example"
📊 Terraform Graph
terraform graph
🔄 Terraform Import
terraform import aws_instance.example i-123456
🔐 Important Notes
Terraform uses API calls
State file contains sensitive data
Always use remote backend (S3 + DynamoDB)
Never commit .tfstate to GitHub
⚔️ Terraform vs Ansible
Terraform	Ansible
Provision infrastructure	Configure servers
Declarative	Procedural

👉 Best practice:
Use Terraform + Ansible together

🔁 Alternatives
AWS CloudFormation
Azure ARM
Pulumi
Ansible
Chef
Puppet

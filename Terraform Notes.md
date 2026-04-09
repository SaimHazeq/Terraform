## Terraform

Terraform is an **Infrastructure as Code (IaC)** tool used to automate infrastructure provisioning.

### 🔹 Key Facts
- Open-source tool
- Platform-independent
- Written in **Go**
- Created by **Mitchell Hashimoto (HashiCorp)** in 2014
- Uses **HCL (HashiCorp Configuration Language)**

---

## ⚙️ How Terraform Works

```text
Code (HCL) → Execute → Infrastructure
🎯 Advantages
Reusable code
Time-saving
Automation
Reduces human errors
Supports dry runs (terraform plan)
☁️ Cloud Alternatives
Tool	Cloud
CloudFormation	AWS
ARM Templates	Azure
Google Deployment Manager	GCP
Terraform	Multi-cloud
🔁 Other Alternatives
Pulumi
Ansible
Chef
Puppet
OpenTofu
⚔️ Terraform vs Ansible
Terraform	Ansible
Creates infrastructure	Configures infrastructure
Declarative	Procedural
🖥️ Installation (Linux)
sudo yum install -y yum-utils shadow-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform

aws configure
📁 Terraform Configuration
File Extension
.tf
Basic Example
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "one" {
  ami           = "ami-03eb6185d756497f8"
  instance_type = "t2.micro"
}
🔑 Core Commands
Command	Description
terraform init	Initialize plugins
terraform plan	Preview changes
terraform apply	Create resources
terraform destroy	Delete resources
⚡ Auto Approve
terraform apply --auto-approve
terraform destroy --auto-approve
📊 Terraform State
Stores infrastructure details
Tracks resource changes
Critical file (terraform.tfstate)

⚠️ Never lose this file

🎯 Target Specific Resources
terraform destroy -target="aws_instance.one"
🧹 Format Code
terraform fmt
📦 Variables
variable.tf
variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "instance_count" {
  type    = number
  default = 2
}
Usage
resource "aws_instance" "one" {
  count         = var.instance_count
  instance_type = var.instance_type
}
📂 TFVARS (Environment Config)
dev.tfvars
instance_count = 1
instance_type  = "t2.micro"
Usage
terraform apply -var-file="dev.tfvars"
💻 CLI Variables
terraform apply -var="instance_type=t2.micro"
🌍 Environment Variables
export TF_VAR_ami=ami-123456
📤 Outputs
output "instance_ip" {
  value = aws_instance.one.public_ip
}
🔄 Taint & Untaint
terraform taint aws_instance.one
terraform untaint aws_instance.one
🔁 Replace Resource
terraform apply -replace="aws_instance.one"
📌 Locals
locals {
  env = "prod"
}
🏗️ Workspaces
terraform workspace list
terraform workspace new dev
terraform workspace select dev
terraform workspace delete dev
☁️ Remote Backend (S3)
terraform {
  backend "s3" {
    bucket = "my-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
🔗 Meta Arguments
depends_on
depends_on = [aws_s3_bucket.example]
count
count = 3
for_each
for_each = toset(["dev", "test", "prod"])
🔄 Lifecycle Rules
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
📦 Modules

Reusable Terraform components.

module "ec2" {
  source = "./modules/ec2"
}
🔁 Dynamic Blocks

Used for looping inside resources.

⚙️ Provisioners
Local Exec
provisioner "local-exec" {
  command = "echo Hello"
}
Remote Exec
provisioner "remote-exec" {
  inline = ["sudo yum update -y"]
}
🧠 Data Sources

Fetch existing infrastructure data.

📥 Import Existing Resources
terraform import aws_instance.example i-123456
🔄 Refresh
terraform refresh

⚠️ Avoid manual usage in production.

📌 Version Constraints
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.0.0"
    }
  }
}
🧾 Maps
variable "tags" {
  type = map(string)
}
📊 Graph
terraform graph
⚠️ Important Notes
Terraform uses API calls → limited & slow
State file contains sensitive data
Always secure your state
Terraform works on desired state model
You can manage manually created resources using import
🎯 Summary

Terraform enables:

Automation
Scalability
Multi-cloud deployments
Consistent infrastructure

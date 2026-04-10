# 🚀 Terraform Notes (Beginner to Advanced)

## 📌 What is Infrastructure?
Infrastructure refers to the resources required to run applications in the cloud.

**Examples:**
- EC2
- S3
- VPC
- ELB
- Auto Scaling Group (ASG)

---

## ❌ Traditional Approach (Manual)
- Time-consuming  
- Error-prone  
- Manual effort  

---

## ✅ Terraform

Terraform is an **Infrastructure as Code (IaC)** tool used to automate infrastructure provisioning.

### 🔹 Key Facts
- Open-source
- Platform-independent
- Written in Go
- Created in 2014 by Mitchell Hashimoto (HashiCorp)
- Uses HCL (HashiCorp Configuration Language)

---

## ⚙️ How It Works
Code (HCL) → Execute → Infrastructure


---

## 🎯 Advantages
- Reusable
- Time-saving
- Automation
- Reduces errors
- Supports dry run (`terraform plan`)

---

## ☁️ Cloud Alternatives

| Tool | Cloud |
|------|------|
| CloudFormation | AWS |
| ARM Templates | Azure |
| Deployment Manager | GCP |
| Terraform | Multi-cloud |

---

## 🔁 Other Alternatives
- Pulumi  
- Ansible  
- Chef  
- Puppet  
- OpenTofu  

---

## ⚔️ Terraform vs Ansible

| Terraform | Ansible |
|----------|--------|
| Creates infrastructure | Configures infrastructure |
| Declarative | Procedural |

---

## 🖥️ Installation (Linux)

```bash
sudo yum install -y yum-utils shadow-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform

aws configure
```

## 📁 Configuration
## File Extension
```
.tf
```
```bash
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "one" {
  ami           = "ami-03eb6185d756497f8"
  instance_type = "t2.micro"
}
```
| Command           | Description        |
| ----------------- | ------------------ |
| terraform init    | Initialize plugins |
| terraform plan    | Preview changes    |
| terraform apply   | Create resources   |
| terraform destroy | Delete resources   |

## ⚡ Auto Approve
```
terraform apply --auto-approve
terraform destroy --auto-approve
```
## 📊 State File
Stores infrastructure data
Tracks changes
File: terraform.tfstate

⚠️ Never lose this file.

## 🎯 Target Resource
```
terraform destroy -target="aws_instance.one"
```
## 🧹 Format
```
terraform fmt
```
## 📦 Variables
```bash
variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "instance_count" {
  type    = number
  default = 2
}
```
```
resource "aws_instance" "one" {
  count         = var.instance_count
  instance_type = var.instance_type
}
```
## 📂 TFVARS
# dev.tfvars
```
instance_count = 1
instance_type  = "t2.micro"
```
```
terraform apply -var-file="dev.tfvars"
```
## 💻 CLI Variables
```
terraform apply -var="instance_type=t2.micro"
```
## 🌍 Environment Variables
```
export TF_VAR_ami=ami-123456
```
## 📤 Outputs
```
output "instance_ip" {
  value = aws_instance.one.public_ip
}
```
## 🔄 Taint / Untaint
```
terraform taint aws_instance.one
terraform untaint aws_instance.one
```
## 🔁 Replace
```
terraform apply -replace="aws_instance.one"
```
## 📌 Locals
```
locals {
  env = "prod"
}
```
## 🏗️ Workspaces
```
terraform workspace list
terraform workspace new dev
terraform workspace select dev
terraform workspace delete dev
```
☁️ Backend (S3)
```
terraform {
  backend "s3" {
    bucket = "my-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
```

## 🔗 Meta Arguments
depends_on
```
depends_on = [aws_s3_bucket.example]
```
count
```
count = 3
```
for_each
```
for_each = toset(["dev", "test", "prod"])
```

## 🔄 Lifecycle
Prevent Destroy
```
lifecycle {
  prevent_destroy = true
}
```
Create Before Destroy
```
lifecycle {
  create_before_destroy = true
}
```
Ignore Changes
```
lifecycle {
  ignore_changes = all
}
```

## 📦 Modules
```
module "ec2" {
  source = "./modules/ec2"
}
```

## 🔁 Dynamic Blocks

Used to reduce repetitive code using loops.

## ⚙️ Provisioners
Local Exec
```
provisioner "local-exec" {
  command = "echo Hello"
}
```
Remote Exec
```
provisioner "remote-exec" {
  inline = ["sudo yum update -y"]
}
```
## 🧠 Data Sources

Used to fetch existing infrastructure data.

## 📥 Import
```
terraform import aws_instance.example i-123456
```
🔄 Refresh
```
terraform refresh
```

⚠️ Avoid using manually in production.

## 📌 Version Constraints
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.0.0"
    }
  }
}
```
## 🧾 Map Variable
```
variable "tags" {
  type = map(string)
}
```
## 📊 Graph
```
terraform graph
```

⚠️ Important Notes
Uses API calls (limited & slow)
State file contains sensitive data
Always secure your state
Works on desired state model
Can manage manual resources using import

## 🎯 Summary

Terraform enables:

Automation
Scalability
Multi-cloud support
Consistent infrastructure

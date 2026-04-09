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
# File Extension
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
```terraform destroy -target="aws_instance.one"
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

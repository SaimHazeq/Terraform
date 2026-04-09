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

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
It will have resource configuration.                                                       
Here we write inputs for our resource.                                                   
Based on that input Terraform will create the real word resource.                      
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
Used to store the resource information which is created by Terraform.                      
To track the resource activities.                                                          
In real time entire resource information is on state file.                                  
We need to keep it sage & secure.                                                           
If we lost this file we can’t track the infrastructure Stores infrastructure data Tracks changes.                                                                                    
File: terraform.tfstate

⚠️ Never lose this file.

## 🎯 Target Resource
```
terraform destroy -target="aws_instance.one"
```
## 🧹 Format
Used to give alignment and indentation for terraform files
```
terraform fmt
```
## 📦 Variables
In real time we keep all the variables in variable.tf to maintain the variables easily.
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
## .tfvars
When we have multiple configuration for terraform to create resources we use tfvars to store different configuration.                                         
On execution time pass the tfvars to the command it will apply the values of that file.
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
Whenever we create a resource by Terraform if you want to print any output of that resources
we can use the output block this block will print the specific output as per out requirement.
```
output "instance_ip" {
  value = aws_instance.one.public_ip
}
```
## 🔄 Taint / Untaint
It is used to recreate specific resources in infrastructure.
Why:                                                                            
If I have an EC2 --> Crashed.                                                  
EC2 --> Code --> main.tf                                                        
Now to recreate this EC2 separately we need to taint the resource.
```
terraform taint aws_instance.one
terraform untaint aws_instance.one
```
## 🔁 Replace
```
terraform apply -replace="aws_instance.one"
```
## 📌 Locals
It’s a block used to define values once you define a value on this block you can use them
multiple times changing the value in local block will be replicated to all resources.
Simply define value once and use for multiple times.
```
locals {
  env = "prod"
}
```
NOTE: Values will be updated when we change them on same workspace.
## 🏗️ Workspaces
It is used to create infrastructure for multiple environment.                    
It will isolate each environment.                                             
If we work on dev env it won’t affect test env.                                 
The default workspace is “default”.                                            
All the resource we create on terraform by default will store on “default”
workspace.                                                                       
All workspace state files will be stored on terraform.tfstate.d folder.
```
terraform workspace list          #to list the workspaces
terraform workspace new dev       #to create workspace
terraform workspace show          #to show current workspace
terraform workspace select dev    #to switch to dev workspace
terraform workspace delete dev    #to delete dev workspace
```
NOTE:
1. We need to empty the workspace before delete.
2. We can’t delete current workspace, we can switch and delete.
3. We can’t delete default workspace.
## ☁️ Backend (S3)
State file is very important in terraform.                                     
Without state file we can’t track the infrastructure.                           
If you lost it we can’t manage the infrastructure.                              
Backup file is a backup of the terraform.tfstate.file                          
Terraform automatically creates a backup of the state file before making any changes to the state file.                                                      
This ensures that you can recover from a corrupted or lost state file.
```
terraform {
  backend "s3" {
    bucket = "my-bucket"
    key    = "terraform.tfstate"
    region = "us-east-1"
  }
}
```
```
terraform state list                                   #to list the resources
terraform state show aws_subnet.two                    #to show specific resource info
terraform state mv aws_subnet.two aws_subnet.three     #to move state info from one to another
terraform state rm aws_subnet.three                    #to remove state information of a resource
terraform state pull                                   #to pull state file information from backend
terraform init -migrate-state
terraform init -reconfigure                            #to bring state file to local
```

## 🔗 Meta Arguments
**depends_on**                                                                     
One resource creation depends on another resource.
Used to manage dependencies of resources.
```
depends_on = [aws_s3_bucket.example]
```
**count**                                                                           
Count is to create identical objects which is having same configuration.
```
count = 3
```
**for_each**
```
for_each = toset(["dev", "test", "prod"])
```

## 🔄 Lifecycle
**Prevent Destroy**                                                                
Used to prevent the resources form destroying.
```
lifecycle {
  prevent_destroy = true
}
```
**Create Before Destroy**                                                            
If we want to recreate any object in terraform. First of all terraform will destroy the existing object and then it will create the new object.             
It will create new replacement object is created first , & destroyed the existing resource.
```
lifecycle {
  create_before_destroy = true
}
```
**Ignore Changes**                                                                  
Whenever we do any changes to the infrastructure manually if I run terraform plan or if I run **terraform apply** the values will be taken to the terraform state if I want to ignore the manual changes made to my infrastructure we can use ignore changes.
```
lifecycle {
  ignore_changes = all
}
```
NOTE:
It is mainly used to ignore the manual changes applied to the infrastructure if you apply any
change to the existing infrastructure manually terraform will completely ignore during the
runtime.
## Providers
Terraform will support thousands of providers in real time but among them we are not going to use some specific providers which is going to maintain by community.
1. OFFICIAL : Maintain by Terraform
2. PARTNER : Maintain by Terraform & That Organization
3. COMMUNITY : Maintain by Individuals
## 📦 Modules
Used for Reusable.                                                             
It divides the code into folder structure.                                     
A module that has been called by another module is often referred to as a child module.                                                                        
We can publish modules for others to use, and to use modules that others have published.                                                                    
These modules are free to use, and Terraform can download them automatically.   
If you specify the appropriate source and version in a module call block.
```
module "ec2" {
  source = "./modules/ec2"
}
```

## 🔁 Dynamic Blocks

It is used to reduce the length of code and used for reusability of code in loop.
## ⚙️ Provisioners
**local Exec**                                                                     
Used to execute commands or scripts in terraform managed resources on both local and remote.
```
provisioner "local-exec" {
  command = "echo Hello"
}
```
**Remote Exec**                                                                    
Used to run the command on remote servers.                                     
Once the server got created it will execute the command and scripts for installing the software’s and configuration them and even for deployment also.
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
## 🔄 Refresh
It will store the values when compared with real world infrastructure when we modified the Terraform values in real world infrastructure it does not replicate to state file so we need to run the command called Terraform Refresh it will refresh the state file while refreshing state file it will compare original values with the state file values if the original values are modified or change it will be replicated to state file after running **terraform refresh**command.

```
terraform refresh
```
When we run **plan**, **apply** or **destroy** refresh will perform automatically.                                                                   
NOTE:                                                                          
Change something manually and check it
DISADVANTAGE: Sometimes it will delete all of the existing infrastructure due to some small sort of changes so in real time we never run this command manually.

⚠️ Avoid using manually in production.

## 📌 Version Constraints
We can change the versions of providers plugins.                               
Whenever we have new changes on the AWS console the old code might not work.     
So if you want to work with the new code window download the new provider plugins for the new code.                                                       
In real time we update the plugins based upon our requirements.
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

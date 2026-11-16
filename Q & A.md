### 1. What is Terraform?  
**Terraform is an Infrastructure as Code tool that allows us to automate infrastructure provisioning using declarative configuration files. I use Terraform to create and mange AWS resources like VPC, EC2, S3, IAM, and EKS. It helps maintain consistency across environments, supports version control, and makes infrastructure management repeatable and scalable.**


### 2. How does Terraform differ from configuration management tools like Ansible? 
**Terraform is mainly used for infrastructure provisioning, while Ansible is used for configuration management. Terraform creates infrastructure resource like EC2, VPC, and S3, whereas Ansible configures servers by installing packages, managing services, and deploying applications. We can use both together: Terraform provisions the infrastructure, and Ansible configures it.** 


### 3. What are the core components of Terraform? 
**Terraform has 5 major core components that work together to provision and manage infrastructure.**    

**1. Terraform Configuration Files**  
**These are files written in HCL(HashiCorp Configuration Language) that define the desired infrastructure.**    
- **File extension: `.tf`**
- **Example: `main.tf`, `variable.tf`, `output.tf`**

**2. Providers**  
**Providers are plugin that allow Terraform to interact with cloud platform and other services. (AWS, Azure, GCP, Kubernetes)**  
```
provider "aws" {
  region = "ap-south-1"
}
```

**3. Resources**  
**Resources are the infrastructure components that Terraform creates, updates, and manages. (EC2, S3 Bucket, VPC, IAM Roles, RDS)**   
```
resource "aws_instance" "web" {
  ami            = "ami-12345678"
  instance_type  = "t2.micro"
}
```

**4. State File**  
**Terraform maintains a state file (`terraform.tfstate`) to track the infrastructure it manages.**  

**It helps Terraform:**  
- **Track existing resource.**
- **Compare actual infrastructure with configuration.**
- **Identify changes.**
- **Determine what needs to be created, updated, or destroyed.**

**5. Modules**
**Modules are reusable collection of Terraform configuration files.**   
**That help:**  
- **Avoid code duplication.**
- **Maintain consistent infrastructure.**
- **Organize Terraform configurations.**
- **Reuse infrastructure across environments.**
**For example, we can create a reusable VPC modules and use it in Dev, QA, and Prod.**


### 4. What is the Terraform workflow?
**1. Write: Define infrastructure in `.tf` files.**  
**2. Initialize: `terraform init` downloads required providers and initialize the backend.**  
**3. Plan: `terraform plan` previews infrastructure changes.**  
**4. Apply: `terraform apply` provisions or modifies infrastructure.**  
**5. State: Terraform updates the state file to track managed resources.**  

**Terraform has several core components, including configuration files, providers, resources, state file, and modules.**  
**Configuration files define the desired infrastructure, Providers interact with cloud platforms, Resource represent infrastructure components, State files track managed resources, and Modules help us create reusable configurations.**  
**Together, these components help automate and manage infrastructure efficiently.**

### 5. What are Terraform plugins?
**Terraform plugins extend Terraform's functionality. Providers are the most important plugins because they allow Terraform to communicate with external platforms like AWS, Azure, GCP, through their APIs. They are responsible for creating, updating, and deleting infrastructure resources.**  

**Terraform downloads the required provider plugins initialization using `terraform init`.**  

**Provisioners are another mechanism that allows us to execute scripts or commands on local or remote machines after a resource is created.**  

### 6. What is the Terraform execution plan?
**Terraform execution plan is a preview of the infrastructure changes Terraform intends to make. We use the `terraform pla,` command to compare the configuration files with the current state and infrastructure.**   
  
**It shows which resource will be created, updated, or destroyed without making actual changes. This help us review changes before applying them to production.**  


### 7. How does Terraform determine resource order?
**Terraform determines resource creation order using a dependency graph called DAG (Directed Acyclic Graph). It identifies dependencies between resources and creates them in the correnct order. If one resource depeds on another, Terraform creates the dependency first.**    
  
**It automatically identifies dependencies through resource reference, know as implicit dependencies.**  
  
**If Terraform cannot identify a dependency automatically, we can define it using `depends_on`, which creates an explicit dependency.**   
**Independent resources can be created in parallel to improve execution speed.**  

### 8. What happens when you run terraform apply? 
**When we run Terraform apply, Terraform reads the configuration files, check the current infrastructure and state, and generates an execution plan. After approval, it builds a dependency graph and executes the required changes through provider APIs. Finally, it updates the state file to reflect the latest infrastructure information. Independent resources can be created in parallel.**  

**1. Configuration Loading**  
- **Terraform reads all `.tf` configuration files.**
- **Loads providers, resources, variables, and modules.**

**2. State Refresh**
- **Terraform checks the current state of managed resources against the real infrastructure to identify existing changes.**

**3. Execution Plan**  
- **Terraform generates an execution plan.**
- **Determines which resource need to be created, updated, or destroyed.**

**4. User Approval**
- **Terraform displays the proposed changes.**
- **By default, it asks for confirmation before proceeding.**

**5. Dependency Graph**
- **Terraform determines the resource execution order using a dependency graph (DAG).**
- **Independent resources can be provisioned in parallel.**

**6. Infrastructure Provisioning**  
- **Terraform communicates with cloud providers APIs through plugins.**
- **Creates, updates, or deletes resource according to the execution plan.**

**7. State File Update** 
- **Terraform updates the state file (`terraform.tfstate`) with the latest information about managed resources.**


### 9. What is terraform.tfstate.backup?    


Why should we use remote state?  
How do you store Terraform state remotely?  
What is state locking?   
Why is state locking important?  
How does Terraform prevent concurrent modifications?  
What is state drift?    
How do you detect infrastructure drift?  
How do you refresh Terraform state?  
How do you remove resources from state?   
What are providers in Terraform?    
How do you use multiple regions in Terraform?    
What is a resource block?   
What is implicit dependency?    
What is explicit dependency?    
What are input variables?   
Why are variables used in Terraform?   
How do you declare variables?    
What is a variable file?   
How do you use environment variables in Terraform?  
How do you use environment variables in Terraform?    
What are provisioners?    
Types of provisioners?   
What is local-exec?   
What is remote-exec?   
Why are provisioners discouraged?   
When should you use provisioners?  
What are data sources?    
What is a backend?   
Types of Terraform backends?   
How do you structure Terraform projects?   
What are Terraform best practices?   
How do you manage secrets in Terraform?    
Should Terraform state contain secrets?  
How do you avoid hardcoding values?   
How do you handle multiple environments?   
How do you integrate Terraform with CI/CD?   
How do you use Terraform with Jenkins?   
What is Terraform automation workflow?   
What are count and for_each?   
Difference between count and for_each?    
What are dynamic blocks?   
What is locals in Terraform?    
What is terraform import?   
What is terraform taint?   
What is terraform untaint?   
How do you secure Terraform state files?    
How do you store secrets in Terraform?    
What permissions should Terraform IAM roles have?   
How do you manage multiple environments (dev, stage, prod)?   
How do you handle infrastructure drift?   
How do you roll back Terraform changes?   
How do you migrate Terraform state from local to remote?     
How do you import existing infrastructure into Terraform?    
How do you handle secrets like DB passwords?    
What happens if someone manually changes infrastructure in AWS?    
How do you share Terraform modules across teams?    
Terraform apply failed halfway. What will you do?    
State file got corrupted. How do you recover?    
Two engineers ran Terraform apply simultaneously. What happens?    
Terraform created duplicate resources. Why?     
Terraform can't detect a change in infrastructure. Why?     
How do you debug Terraform errors?
Tell me about a Terraform project you worked on.
How did you manage Terraform state in production?
How did you structure Terraform code for large infrastructure?
How do you review Terraform code in a team?
How do you prevent accidental infrastructure deletion?
How do you test Terraform code?

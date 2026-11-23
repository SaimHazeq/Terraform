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

  
**"Terraform has several core components, including configuration files, providers, resources, state file, and modules.**  
**Configuration files define the desired infrastructure, Providers interact with cloud platforms, Resource represent infrastructure components, State files track managed resources, and Modules help us create reusable configurations.**  
**Together, these components help automate and manage infrastructure efficiently."**


### 4. What is the Terraform workflow?
**1. Write: Define infrastructure in `.tf` files.**  
**2. Initialize: `terraform init` downloads required providers and initialize the backend.**  
**3. Plan: `terraform plan` previews infrastructure changes.**  
**4. Apply: `terraform apply` provisions or modifies infrastructure.**  
**5. State: Terraform updates the state file to track managed resources.**  



### 5. What are Terraform plugins?
**Terraform plugin are executable components that extend Terraform's functionality and allow it to interact with external platforms, services, and tools.**  
**Terraform uses plugins to communicate with cloud providers and manage infrastructure resources.**  

**Types of Terraform Plugins (There are 3 main types)**   
  
**1. Providers**
- **Providers allow Terraform to interact with external platforms and APIs.**
- **They are responsible for creating, updating, and deleting infrastructure resources. (Example: AWS, Azure, GCP, K8s).**

**2. Provisioners**  
- **Provisioners execute scripts or commands on local or remote machines after a resource is created.**
- **Example: `local-exec` and `remote-exic`**

**3. Third-party Providers**
- **These are providers developed and maintained by organizations or the community.**
- **Example include providers for Datadog, Cloudflare, and GitHub.**  
  
**Terraform downloads the required provider plugins initialization using `terraform init`.**  


### 6. What is the Terraform execution plan?
**Terraform execution plan is a preview of the infrastructure changes Terraform intends to make. We use the `terraform plan,` command to compare the configuration files with the current state and infrastructure.**   
  
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
**`terraform.tfstate.backup` is a backup of the previous Terraform state file.**  
**Whenever Terraform updates the local state file, it generally creates a backup of the previous version.**  
**It helps us recover previous state information if something goes wrong.**  


### 10. Why should we use remote state?  
**We use remote state to store Terraform state file centrally instead of keeping them on local machines.**  
**It helps with team collaboration, state locking, security, and backup recovery.**  
**For example, in AWS, we use an S3 bucket as a remote backend with encryption and versioning enabled. This allows multiple DevOps engineers to work with consistent infrastructure state while preventing conflicting modifications through state locking.**

  
### 11. What is state locking? 
**State locking is a mechanism that prevents multiple users from modifying the same Terraform state simultaneously.**  
**For example, if two DevOps engineers run Terraform apply at the same time, Terraform allows one operation to acquire the lock while preventing the other from modifying the state.**   

  
### 12. What is state drift?
**State drift occurs when actual infrastructure differs from the Terraform configuration or recorded state, usually because someone makes manual changes outside Terraform.**  
**For example, if Terraform creates an EC2 instance with t2.micro and someone manually changes it to t3.medium through the AWS Console, drift occurs.**  
  
**We can detect it using `terraform plan` and fix it by either reverting the manual change or updating the Terraform configuration to match the intended infrastructure.**

     
### 13. How do you remove resources from state?  
**We use `terraform state rm` to remove a resource from Terraform state without deleting the actual infrastructure.**    
  
**For example, if we run `terraform state rm aws_instance.<name>`, Terraform stop tracking that EC2 instance, but it continues running in AWS.**  

**Command Difference**   
```
terraform state rm     #Removers resource tracking from state
terraform destroy      #Destroy actual infrastructure
terraform state mv     #Moves or renames a resource address in state
terraform import       #Imports an existing resource into Terraform state
```

   
### 14. How do you use multiple regions in Terraform? 
**We can manage multiple AWS regions in Terraform using provider aliases.**  
**For example, we configure Mumbai as the default provider and Singapore as an additional provider using an alias.**    
   
**Then, while defining resources, we specify the appropriate provider using the provider argument.**  
**This allows us to provision and manage infrastructure across multiple AWS regions using a single Terraform configuration.**  
```
provider "aws" {
  alias     = "Mumbai"
  region    = "ap-south-1"
}

provider "aws" {
  alias     = "Singapore"
  region    = "ap-southeast-1"
}

resource "aws_instance" "Web" {
  provider         = "aws.Mumbai"
  ami              = "ami-1234578"
  instance_type    = "t2.micro"
}

resource "aws_s3_bucket" "my-bucker" {
  provider         = "aws.Singapore"
  bucket           = "my-bucket-1"
}
```
    
### 15. What are input variables?  
**Input variables in Terraform allows us to make infrastructure configurations dynamic and reusable.**  
**Instead of hardcoding value, we define variables in variable.tf and reference them using `var.varibale_name`.**  
**We can pass values through default settings, command-line arguments, terraform.tfvars files, or environment variables.**    
```
variable "instance_type" {
  description  = "EC2 instance type"
  type         = strign
  default      = "t2.micro"
```


### 16. Why are variables used in Terraform? 
**We use variables in Terraform to make infrastructure code reusable, flexible, and maintainable.**  
**Instead of hardcoding values, we define input variables and pass different values for different environments.**  

**For example, we can use the same Terraform configuration to deploy DEV, QA, and PROD environments with different EC2 instance types, regions, and network configuration.**  

   
### 17. How do you use environment variables in Terraform? 
**We use environment variables in Terraform by defining input variables and passing their values through environment variables with the `TF_VAR_` prefix.**  

**For example, `export TF_VAR_instance_type=t3.medium`. Terraform automatically picks up this value and uses it wherever we reference `var.instance_type`.**  

**This is useful in CI/CD pipelines like Jenkins, where we can dynamically pass environment-specific values without hardcoding them.**  

      
### 18. Why are provisioners discouraged? 
**Provisioners are discouraged in Terraform because the introduce imperative scripts into a declarative infrastructure workflow.**  
**They can cause reliability issues due to SSH connectivity, script failures, and dependency problems.**  

**Instead, we prefer cloud-init or EC2 User Data for initial configuration and Ansible for configuration management. This makes infrastructure deployment more predictable, maintainable, and reliable.**    


### 19. What are data sources?  
**Data sources in Terraform are used to retrieve information about existing infrastructure or external resources without creating or managing them.**  

**For example, if I want to launch an EC2 instance using the latest Amazon Linux AMI, I can use the `aws_ami` data source to dynamically fetch the AMI ID instead of hardcoding it.**  

### 20. How do you structure Terraform projects?        
**I generally structure Terraform projects using a modular approach. I maintain separate directories for reusable modules like VPC, EC2, and Security groups, and separate environment configurations for Dev, QA, and Prod.**  

**I use different variables files and remote state configurations for each environments. This approach avoids code duplication, improves maintainability, and ensures consistent infrastructure deployment.**  

**I also maintain Terraform code in Git and use Jenkins pipelines for automated deployments.**    

**Important: For production environments, I prefer separate state files for Dev, QA, and Production rather than sharing one state file across all environments. This provides better isolation and reduces the risk of accidental changes.**  
```
terraform-project/
│
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   ├── ec2/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   │
│   └── security-group/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
│
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   │
│   ├── qa/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   └── backend.tf
│   │
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       ├── terraform.tfvars
│       └── backend.tf
│
└── README.md
```
**Purpose of Each File**  
**`main.tf`: Defines infrastructure resources**
**`variables.tf`: Declares input variables**  
**`outputs.tf`: Defines output values**    
**`providers.tf`: Configures cloud providers**    
**`terraform.tfvars`: Stores environments-specific variable values**  
**`backend.tf`: Configures remote state storage**  
**`versions.tf`: Defines Terraform and provider version constraints**


### 21. What are Terraform best practices?   
**1. Use reusable modules for common infrastructure.**
**2. Maintain separate configurations and state files for different environments.**  
**3. Use remote backends such as AWS S3.** 
**4. Enable state locking and S3 versioning.**  
**5. Store Terraform code in Git.**  
**6. Pin Terraform and provider versions.**  
**7. Never commit state files or credentials to Git.**  
**8. Use CI/CD pipelines such as Jenkins for Terraform deployments.**  
**9. Use meaningful resource and variable names.**  


### 22. How do you manage secrets in Terraform? 
**I manage secrets in Terraform using AWS Secrets Manager, Parameter Store, and secure CI/CD credentials. I avoid hardcoding passwords or API keys and use sensitive variables to prevent accidental exposure in CLI output.**  

**Since sensitive values can still be stored in Terraform state, I secure the remote state using S3 encryption with KMS, versioning, and restricted IAM permissions.**   

**Terraform can retrieve secrets using a data source**  
```
data "aws_secretmanager_secret_version" "db_password" {
  secret_id = "production/db-password"
}
```

**In Jenkins, I use Credentials Manager to inject secrets securely during pipeline execution.**  


### 23. Should Terraform state contain secrets? 
**Ideally, Terraform state should not contain secrets because it may expose sensitive information like database passwords and API keys.**  

**However, Terraform can store sensitive values in its state file even when variables are marked as sensitive.**  

**I prefer AWS Secret Manager for storing secrets and secure the Terraform state using S3 encryption with KMS, versioning, and restricted IAM access. I also ensure state files are never committed to Git.**   

**Remember: `sensitive = true` hides secrets from normal CLI output; it does not prevent them from being stored in Terraform state.**   

  
### 24. How do you handle multiple environments?   
**In Terraform, we manage multiple environments like Dev, QA, Staging, and Production by using reusable modules, separate variable files, and separate state files.**  

**This allows us to maintain the same infrastructure code while using different configurations for each environment.**  


### 25. How do you integrate Terraform with CI/CD?  
**I integrate Terraform with Jenkins CI/CD to automate infrastructure provisioning.**  

**Whenever a developer pushes Terraform code to GitHub, Jenkins triggers a pipeline that checks out the code, runs `terraform init` and `terraform validate`, and generates an execution plan using `terraform plan`.**  

**After reviewing and approving the plan, Jenkins executes terraform apply to provision or update AWS infrastructure.**  

**I also use an S3 remote backend with state locking, secure AWS credentials, and separate state files for different environments.**    

   
### 26. What are count and for_each? 
**Both `count` and `for_each` are Terraform meta-arguments used to create multiple resources.**  

**1. `count`:**  
- **Uses a numeric to determine how many resource instances to create.**
- **Each instance is identified by a numeric index starting from 0.**
```
resource "aws_instance" "web" {
  count = 3
  ami = "ami-12345678"
  instance_type = "t3.micro"
```

**2. `for_each`:**  
- **Uses a map or set of strings to create multiple resource instances.**
- **Each instance is identified by a unique key.**
- **Allows different configurations for each instances.**
```
resource "aws_instance" "web" {
  for_each = {
    dev    = "t3.micro"
    test   = "t3.small"
    prod   = "t3.medium"
  }

  ami             = "ami=1234578"
  instance_type   = each.value

  tags = {
    Name = each.key
  }
}
```


     
### 27. What are dynamic blocks? 
**Dynamic block in Terraform are used to generate repeated nested blocks dynamically.**    
  
**They work with `for_each` to iterate over a list or map and generate configuration using the `content` block.** vv
  
**For example, I can use dynamic blocks to create multiple ingress rules in an AWS security group without manually writing each rule.**  
```
variable "ingress_rules" {
  default = [
    }
      port = 80
    },
    {
      port = 443
    }
  ]
}

resource "aws_security_group" "web" {
  name = "web-sg"

  dynamic "ingress" {
    for_each = var.ingress_rules

    content {
      from_port    = ingress.value.port
      to_port      = ingress.value.port
      protocol     = "tcp"
      cide_blocks  = ["0.0.0.0/0"]
    }
  }
}
```


### 28. What is locals in Terraform?  
**Locals in Terraform are used to define reusable values and expressions within a configuration. They help avoid code duplication and simplify complex expressions.**  

**For example, We use locals to define common resource tags, naming conventions, and environment-specific values. We reference them using `local.variable_name`.**  
```
locals {
  project_name  = "my-app"
  environment   = "dev"
  common_tags = {
    project      = "my-app"
    Environment  = "dev"
    ManagedBy    = "Terraform"
  }
}

resource "aws_instance" "web" {
  ami            = "ami-1234578"
  instance_type  = "t3.micro"

  tags  = local.common_tags
```


### 29. What is terraform import? 
**Terraform import is used to bring existing infrastructure under Terraform management without recreating it.**  

**For example, if an EC2 instance was created manually in AWS, We can import it using its instance ID. Terraform adds the resource association to the state file, and then we write or verify the configuration and run a plan to ensure everything matches.**    
```
terraform import aws_instance.web i-12345789abcdefg

#Using `import` block:
import {
  to  = aws_instance.web
  id  = "i-12345789abcdefg
}
```


### 30. What is terraform taint?  
**`terraform taint` was used to mark a resource for forced replacement during the next apply.**     
  
**For example, if an EC2 instance was corrupted, We could mark it for recreation. However, this command is deprecated, and the modern approach is to use `terraform plan -replace` and `terraform apply -replace`.**  
```
terraform taint aws_instance.<name>
terraform apply
  
terraform plan -replace="aws_instance.<name>"
terraform apply -replace="aws_instance.<name>"
```

### 31. What is terraform untaint?  
`terraform untaint` was used to remove the tainted status from a resource, preventing Terraform from replacing it during the next apply.**  

**For example, if we accidentally marked an EC2 instance for replacement but later confirmed it was healthy, we could use untaint to cancel that replacement. This command is now deprecated along with terraform taint.**    
  
`
terraform untaint aws_instance.<name>  
`
   
### 32. What permissions should Terraform IAM roles have?   
**I follow the principle of least privilege when configuring IAM roles for Terraform. I provide only the permission required to manage the AWS services defined in the Terraform configuration.**  

**For example, if Terraform manages EC2, VPC and S3, I grant the necessary permissions for those services.**  

**In CI/CD, such as Jenkins, I prefer using an IAM roles with temporary credentials through STS rather than storing permanent access keys. I avoid AdministratorAccess in production.**  


### 33. How do you roll back Terraform changes?
**Terraform doesn't provide automatic rollback like a database. If a change causes an issue, I usually revert the Terraform configuration to the previous version, run `terraform plan` to review the changes, and then apply it to restore the previous infrastructure configuration.**  

**If the state file is corrupted, I can recover it using S3 versioning, after verifying it against the actual infrastructure. I also maintain Git history and S3 state backups to support recovery.**  

**Important: Reverting Terraform code does not guarantee that the infrastructure can return to its exact previous state. Some changes are destructive or irreversible, so recover must be planned carefully.**  


### 34. How do you migrate Terraform state from local to remote?  
**First, I create an S3 bucket with encryption and versioning enabled. Then I configure the S3 backend in Terraform with the bucket name, state file key, and region.**  

**I run `terraform init =migrate-state`, which migrates the existing local state file to S3 after confirmation. Finally, I run `terraform state list` and `terraform plan` to verify that all existing resources are recognized and so unexpected changes are proposed.**       
   
### 35. What happens if someone manually changes infrastructure in AWS?  
**If someone manually changes infrastructure in AWS outside Terraform, it creates configuration drift.**     

**We detect it by running `terraform plan`, which refreshes the actual infrastructure information and identifies differences. If the change is unauthorized, we revert it using Terraform.**  

**If it's intentional, we update the Terraform configuration to match the desired infrastructure.**  

**We also use IAM restrictions, AWS Config and CI/CD pipelines to minimize manual changes.**  


### 36. How do you share Terraform modules across teams? 
**We share Terraform modules across teams by maintaining reusable modules in a centralized Git repository or private Terraform Registry.** 

**For example, we can create standard modules for VPC, EC2, security groups and EKS. Other teams can consume these modules by referencing their Git repository and specific version.**  


### 37. Terraform apply failed halfway. What will you do?
**If Terraform apply fails halfway, I first check the error logs to identify the failed resource and root cause.**    

**Common reasons:**
- **IAM permission issue.**
- **Resource quota limits.**
- **AWS API errors.**
- **Dependency failures.**
- **Network connectivity issues.**

**I verify the Terraform state using `terraform state list` and compare it with the actual AWS infrastructure. After fixing the issue, I run `terraform plan` to review the remaining changes and execute `terraform apply` again.**  

**Terraform doesn't provide automatic rollback, so I carefully verify partially created resources before retrying.**  

    
### 38. Two engineers ran Terraform apply simultaneously. What happens? 
**If two engineers run Terraform apply simultaneously against the same remote state, Terraform state locking prevents concurrent operations.**  

**The first engineer acquires the lock and proceeds, while the second engineer waits or receives a lock error. Once the first operation completes, the second engineer can retry.**  

**In AWS, I use S3 native state locking with `use_lockfile = true` to prevent concurrent state modifications.**  

### 39. Terraform created duplicate resources. Why?
**Terraform can create duplicate resource when existing infrastructure is not properly tracked in its state file.**  

**Common reasons include manual resource creation, missing imports, multiple state files managing the same infrastructure, or incorrect resource configuration.**  

**I troubleshoot this b checking `terraform state list`, comparing it with actual AWS resources, and reviewing `terraform plan`.**  
**Then I import existing resources where appropriate, fix the configuration, and apply only after verifying the plan.**    

**Important: Remote state and locking alone do not prevent two separate Terraform states from managing the same AWS resource. You need clear ownership of resource and state files.**  


### 40. Terraform can't detect a change in infrastructure. Why?
**Terraform may fail to detect infrastructure changes if refresh is disabled, the resource isn't managed in Terraform state, the provider cannot detect a particular attribute, or `ignore_changes` is configured.**  

**I troubleshoot by running `terraform plan` with refresh enabled, checking the state, reviewing lifecycle rules, and verifying provider capabilities and AWS API permissions.**     

**Common Reasons**
- **Refresh is disabled: `terraform plan -refresh=false`**
- **Provider limitations: Some AWS resource attributes cannot be refreshed or detected reliably by the provider.**
- **Resource is not managed by Terraform: If a resource is created manually and hasn't been imported, Terraform doesn't track it and won't detect its changes as drift.**
- **Changes to ignored attributes: If you use `ignore_changes` in a lifecycle block, Terraform intentionally ignores changes to specified attributes.**
```
lifecycle {
  ignore_changes  = [
    tags
  ]
}
```
- **Eventual consistency or API issues: Sometimes cloud provider APIs take time to reflect changes, or API errors prevent Terraform from retrieving the latest information.**
  

### 41. How do you debug Terraform errors?
**When debugging Terraform errors, I first analyze the error message to identify the failing resource and root cause.** 

**Then I run `terraform fmt` and `terraform validate` to check the configuration, followed by `terraform plan` to review the proposed changes.**  

**If necessary, I enable debug logging using `TF_LOG=DEBUG`. I also inspect Terraform state, verify AWS IAM permissions and provider versions, fix the issue, and run plan again before applying.**  

**Common Errors:**  
- **`AccessDenied`: Insufficient IAM permissions**
- **`InvalidAMIID.NotFound`: Incorrect or region-specific AMI ID**
- **`Error acquiring the state lock`: Another Terraform operation holds the lock**
- **`Resource already exists`: Resource exists but isn't properly tracked**
- **`Unsupported argument`: Invalid argument for the resource or provider version**
- **`Dependency cycle`: Circular resource dependencies**



### 44. Tell me about a Terraform project you worked on.
**I worked on an AWS infrastructure automation project where we used Terraform to provision and manage cloud infrastructure across Dev, QA, and Production environments.**  

**I used Terraform to create AWS resources such as VPCs, Public and Private subnets, Internet Gateway, NAT Gateway, route tables, security groups, EC2 instances, Application Load Balancer, and IAM roles.**  

**I developed reusable Terraform modules for VPC, EC2, and securit groups to avoid code duplication and maintain consistency across environments.**  

**For state management, I configured an S3 remote backend with encryption, versioning, and state locking to support team collaboration.**  

**I also integrated Terraform with Jenkins CI/CD. Whenever infrastructure code was pushed to GitHub, Jenkins executed Terraform initialization, formatting, validation, and planning. After reviewing the plan and getting approval, we applied the changes.**   

**We also used variables, locals, outputs, and environment-specific configuration files to make the infrastructure reusable and maintainable.**  

**The main objective was to reduce manual infrastructure provisioning, maintain consistency across environments, and improve deployment reliability.**  


### 45. How did you structure Terraform code for large infrastructure?
**For a large infrastructure, I follow a modular Terraform architecture. I divide infrastructure into reusable modules such as VPC, EC2, EKS, IAM, and Security groups.**  

**I maintain separate environment configurations for Dev, QA, and Production, with independent remote state files in S3. I also use variables, outputs, version constraints, and Git for maintainability.**  

**Finally, I integrate Terraform with Jenkins to automate validation, planning, approval, and deployment.**    

**Best Practices**
- **Use variables instead of hardcoding values.**
- **Use locals for common configuration and tags.**
- **Use outputs to share information between modules.**
- **Pin Terraform and provider version.**
- **Follow consistent naming conventions.**
- **Store code in Git.**
- **Use code reviews before merging changes.**
- **Integrate Terraform with Jenkins CI/CD.**



### 46. How do you review Terraform code in a team?
**In my team, I review Terraform code through Git Pull request. First, I check code formatting, validation, module structure, variables, and provider versions. Then I review the Terraform plan carefully to identify unexpected resource changes, replacements, or deletions.**  

**I also use tools like Checkov and TFLint for security and configuration checks.**  

**Finally, after approval, the changes are merged and deployed through Jenkins CI/CD.**   

**Best Practices**
- **Reusable modules are used.**
- **No hardcoded credentials or secrets.**
- **Least-privilege IAM permissions.**
- **Correct variable and output definitions.**
- **Remote state and locking configuration.**
- **Terraform and provider versions are pinned.**

**Tools**
- **Checkov: Detects infrastructure security misconfigurations.**
- **TFLint: Identifies Terraform configuration issues.**
- **Trivy: Scans IaC configurations for security risks.**


### 47. How do you prevent accidental infrastructure deletion?
**To prevent accidental infrastructure deletion, I use Terraform's `prevent_destroy = true` lifecycle rule for critical resources like production databases.**  

**I also review Terraform plans before applying changes, implement manual approval in Jenkins for production deployments, and restrict deletion permissions through IAM.**  

**Additionally, I enable AWS service-level deletion protection and maintain remote state backups in S3.**  
```
resource "aws_db_instance" "production" {
  identifier  = "prod-database"

  lifecycle {
    prevent_destroy  = true
  }
}
```

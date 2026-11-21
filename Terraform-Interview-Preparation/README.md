# Terraform – DevOps Interview Questions and Answers

## Contents

1. [Terraform Basics](#terraform-basics)
2. [Terraform State Management](#terraform-state-management)
3. [Loops and Modules](#loops-and-modules)
4. [Multiple Environments and Security](#multiple-environments-and-security)
5. [Troubleshooting and Real-Time Scenarios](#troubleshooting-and-real-time-scenarios)
6. [Quick Revision Commands](#quick-revision--important-terraform-commands)

---

# Terraform Basics

## 1. What is Terraform and why is it used?

**Answer:**

Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp. We use it to create and manage infrastructure through code instead of creating everything manually in a cloud console.

For example, we can use Terraform to create AWS EC2 instances, VPCs, Security Groups, Load Balancers, S3 buckets and RDS databases.

**Why we use it:**

- Automates infrastructure provisioning.
- Makes infrastructure reusable and consistent.
- Allows us to maintain code in Git.
- Helps manage infrastructure changes.
- Supports multiple cloud providers.

## 2. What are the key features of Terraform?

**Answer:**

The main features are:

1. **Infrastructure as Code:** Infrastructure is defined in configuration files.
2. **Declarative approach:** We describe the desired infrastructure.
3. **Multi-cloud support:** Terraform supports AWS, Azure, GCP and other platforms.
4. **Reusable modules:** We can reuse common infrastructure code.
5. **State management:** Terraform tracks resources it manages.
6. **Dependency management:** Terraform identifies dependencies between resources.
7. **Execution plan:** We can review proposed changes before applying them.
8. **Version control:** Terraform code can be maintained in Git.

## 3. How does Terraform work and what is its architecture?

**Answer:**

First, we write Terraform configuration files using HCL. Terraform initializes the working directory and downloads the required providers.

Then we run `terraform plan` to preview the changes. After reviewing the plan, we run `terraform apply`.

Terraform communicates with the cloud provider API to create or update resources and records managed resource information in state.

**Terraform workflow:**

```text
Terraform Configuration (.tf files)
              |
              v
        terraform init
              |
              v
        terraform plan
              |
              v
        terraform apply
              |
              v
      Cloud Infrastructure
```

**Important commands:**

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
```

## 4. What is the difference between Infrastructure as Code (IaC) and traditional provisioning?

**Answer:**

Traditional provisioning means creating infrastructure manually through a cloud console.

Infrastructure as Code means defining infrastructure in configuration files and using tools such as Terraform to create and manage it.

| Traditional Provisioning | Infrastructure as Code |
|---|---|
| Mostly manual | Automated through code |
| Console-based | Configuration-based |
| Harder to repeat consistently | Easier to repeat consistently |
| Changes may not be tracked centrally | Code can be version-controlled |
| More manual effort | Less repetitive manual effort |

## 5. Explain the Terraform workflow: init, plan, apply and destroy.

**Answer:**

Terraform has four important commands.

**1. terraform init**

Initializes the working directory, downloads providers and initializes the configured backend.

```bash
terraform init
```

**2. terraform plan**

Shows the changes Terraform proposes to make.

```bash
terraform plan
```

**3. terraform apply**

Applies the proposed changes to infrastructure.

```bash
terraform apply
```

**4. terraform destroy**

Destroys resources managed by the current configuration and state.

```bash
terraform destroy
```

**Easy way to remember:**

- Init = Initialize
- Plan = Preview
- Apply = Execute
- Destroy = Remove

## 6. What is the purpose of .tf, .tfvars and .terraform files?

**Answer:**

- **`.tf` files:** Contain Terraform configuration, such as resources, providers, variables and outputs.
- **`.tfvars` files:** Provide values for input variables, often for environments such as DEV, QA and PROD.
- **`.terraform/` directory:** Contains initialized working-directory data, including downloaded providers and module information.
- **`terraform.tfstate`:** Stores information about infrastructure managed by Terraform.
- **`.terraform.lock.hcl`:** Records selected provider versions and checksums.

**Example project structure:**

```text
terraform-project/
│
├── main.tf
├── provider.tf
├── variables.tf
├── outputs.tf
├── dev.tfvars
├── prod.tfvars
├── .terraform.lock.hcl
└── .terraform/
```

Do not commit credentials or sensitive state files to a public Git repository. Use an appropriate `.gitignore` and secure remote state storage.

## 7. What is the difference between resource, data source, variable, local and output?

**Answer:**

| Block | Purpose |
|---|---|
| `resource` | Creates and manages infrastructure |
| `data` | Reads information about existing infrastructure |
| `variable` | Accepts input values |
| `locals` | Defines reusable named expressions |
| `output` | Displays or exposes useful values |

**Example:**

```hcl
variable "instance_type" {
  type    = string
  default = "t3.micro"
}

locals {
  instance_name = "roboshop-mongodb"
}

resource "aws_instance" "mongodb" {
  ami           = "ami-12345678"
  instance_type = var.instance_type

  tags = {
    Name = local.instance_name
  }
}

output "public_ip" {
  value = aws_instance.mongodb.public_ip
}
```

## 8. How do you define and use variables in Terraform?

**Answer:**

Variables accept input values and help us avoid hardcoding values in multiple places.

**variables.tf:**

```hcl
variable "instance_type" {
  type        = string
  default     = "t3.micro"
  description = "EC2 instance type"
}
```

**main.tf:**

```hcl
resource "aws_instance" "mongodb" {
  ami           = "ami-12345678"
  instance_type = var.instance_type
}
```

We can provide values through:

1. Default values.
2. `terraform.tfvars`.
3. Other `.tfvars` files.
4. Environment variables.
5. Command-line arguments.

**Command-line example:**

```bash
terraform apply -var="instance_type=t3.small"
```

## 9. How do you use local values in Terraform and why?

**Answer:**

Locals are named values used to simplify expressions and avoid repeating the same logic.

We can use variables, functions and other local values inside a `locals` block.

Locals cannot be directly overridden like input variables.

**Example:**

```hcl
variable "project" {
  default = "roboshop"
}

variable "environment" {
  default = "dev"
}

locals {
  instance_name = "${var.project}-${var.environment}-mongodb"
}
```

**Result:**

```text
roboshop-dev-mongodb
```

Locals are useful for resource names, common tags and calculated values.

## 10. What is the difference between terraform plan and terraform apply?

**Answer:**

`terraform plan` previews the changes Terraform intends to make. It normally does not create or modify infrastructure.

`terraform apply` executes the planned changes against the provider.

**Example:**

```bash
terraform plan
terraform apply
```

If I add an EC2 resource, plan shows that Terraform intends to create it. After reviewing the plan, I run apply to create the instance.

**Remember:**

- Plan = Preview.
- Apply = Execute.

---

# Terraform State Management

## 11. What is Terraform State and what information does it store?

**Answer:**

Terraform state is like memory for Terraform.

It stores information about the resources Terraform manages and maps Terraform configuration to real infrastructure.

The default local state filename is:

```text
terraform.tfstate
```

For example, after creating an EC2 instance, Terraform records details such as its resource address, instance ID and other attributes in state.

When configuration changes, Terraform uses state together with refreshed provider information to determine the required actions.

**Important:** State can contain sensitive information. It must be protected.

## 12. How do you manage Remote State in Terraform? (S3 + DynamoDB)

**Answer:**

In a team environment, multiple engineers need to work with shared Terraform state.

Instead of keeping state only on each engineer's machine, we can store it centrally in an S3 bucket.

Modern Terraform S3 backends support native S3 lock files. Older configurations commonly used DynamoDB for state locking.

**Example: S3 backend with native locking**

```hcl
terraform {
  backend "s3" {
    bucket       = "roboshop-terraform-state"
    key          = "dev/terraform.tfstate"
    region       = "ap-south-1"
    encrypt      = true
    use_lockfile = true
  }
}
```

Initialize the backend:

```bash
terraform init
```

**Advantages:**

- Centralized state.
- Team collaboration.
- Locking to prevent conflicting operations.
- Access control and backup options.

## 13. Why do we use a backend in Terraform? Explain with an example.

**Answer:**

A backend tells Terraform where and how to store its state.

By default, Terraform uses a local backend and stores state in the working directory.

For team projects, we can use a remote backend such as AWS S3.

**Example:**

```hcl
terraform {
  backend "s3" {
    bucket = "roboshop-state"
    key    = "dev/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

Other backend examples include:

- Local
- S3
- AzureRM
- GCS
- Terraform Cloud

**Remember:** Backend = Where Terraform stores state.

## 14. What is State Locking and how does it prevent conflicts?

**Answer:**

State locking prevents conflicting Terraform operations from modifying the same state at the same time.

For example, if Engineer 1 is running `terraform apply`, Engineer 2 should not perform another conflicting operation against the same state simultaneously.

The backend acquires a lock for the operation and releases it when the operation completes.

If locking is supported and configured, it helps protect shared state from concurrent changes.

## 15. How do you import existing infrastructure into Terraform?

**Answer:**

Sometimes infrastructure is created manually through the AWS Console. If we want Terraform to manage that resource, we can import it into Terraform state.

First, define the matching resource block in configuration:

```hcl
resource "aws_instance" "mongodb" {
  # Add the matching configuration
}
```

Then import the existing EC2 instance:

```bash
terraform import aws_instance.mongodb i-0123456789abcdef0
```

After importing, check the configuration and run:

```bash
terraform plan
```

Import associates the existing resource with Terraform state. It does not automatically create complete Terraform configuration.

Modern Terraform also supports declarative `import` blocks.

```hcl
import {
  to = aws_instance.mongodb
  id = "i-0123456789abcdef0"
}
```

## 16. What is the difference between terraform import and terraform refresh?

**Answer:**

**Import:**

Brings an existing resource under Terraform state management.

**Refresh:**

Updates Terraform's knowledge of the current attributes of resources it already manages.

For example, if an EC2 instance already exists but is not in Terraform state, we can import it.

If someone changes a managed resource manually in AWS, Terraform can refresh its information from AWS.

Modern Terraform workflows generally use:

```bash
terraform plan -refresh-only
terraform apply -refresh-only
```

The standalone `terraform refresh` command is deprecated.

---

# Loops and Modules

## 17. Explain the difference between count and for_each with examples.

**Answer:**

Both are used to create multiple resources.

### count

Count uses a number and creates resources with numeric indexes.

**Example:**

```hcl
resource "aws_instance" "mongodb" {
  count         = 3
  ami           = "ami-12345678"
  instance_type = "t3.micro"

  tags = {
    Name = "mongodb-${count.index}"
  }
}
```

Resource addresses:

```text
aws_instance.mongodb[0]
aws_instance.mongodb[1]
aws_instance.mongodb[2]
```

### for_each

for_each iterates over a map or set and uses keys to identify resources.

**Example:**

```hcl
variable "instances" {
  default = {
    mongodb = "t3.micro"
    redis   = "t3.small"
    mysql   = "t3.medium"
  }
}

resource "aws_instance" "roboshop" {
  for_each      = var.instances
  ami           = "ami-12345678"
  instance_type = each.value

  tags = {
    Name = each.key
  }
}
```

**Difference:**

- `count` uses numeric indexes.
- `for_each` uses meaningful keys.
- `for_each` is useful when resources have different configurations or identities.

## 18. How do you create reusable and modular code in Terraform?

**Answer:**

We create reusable code using Terraform modules.

For example, if multiple projects need EC2 instances and Security Groups, instead of writing the same configuration repeatedly, we create reusable EC2 and Security Group modules.

Then we call those modules from different projects by passing different input values.

This reduces code duplication and makes maintenance easier.

## 19. What is a Terraform module and how do you use it?

**Answer:**

A Terraform module is a collection of Terraform configuration files used together.

I think of a module like a function in programming: we write common code once, pass inputs and reuse it.

**Example structure:**

```text
terraform/
│
├── main.tf
│
└── modules/
    └── ec2/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

**Module resource:**

```hcl
resource "aws_instance" "main" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name = var.instance_name
  }
}
```

**Calling the module:**

```hcl
module "mongodb" {
  source = "./modules/ec2"

  ami_id        = "ami-12345678"
  instance_type = "t3.micro"
  instance_name = "roboshop-mongodb"
}
```

We can call the same module again for Redis with different values.

## 20. Explain the difference between module sources: local path, Git and Terraform Registry.

**Answer:**

Terraform modules can come from different locations.

### 1. Local path

```hcl
module "ec2" {
  source = "./modules/ec2"
}
```

The module is stored inside our project.

### 2. Git source

```hcl
module "ec2" {
  source = "git::https://github.com/example/terraform-aws-ec2.git?ref=v1.0.0"
}
```

The module is downloaded from a Git repository. Pinning a Git reference helps keep module versions controlled.

### 3. Terraform Registry

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.21.0"
}
```

The module is downloaded from Terraform Registry.

---

# Multiple Environments and Security

## 21. How do you manage multiple environments (DEV, QA, PROD) using Terraform?

**Answer:**

In real-time projects, we maintain environments such as DEV, QA, UAT and PROD.

Common approaches are:

1. Terraform workspaces.
2. Separate tfvars files with separate backend configurations.
3. Separate root configurations or repositories.

**Example files:**

```text
dev.tfvars
qa.tfvars
prod.tfvars
```

**DEV configuration:**

```hcl
environment   = "dev"
instance_type = "t3.micro"
```

**PROD configuration:**

```hcl
environment   = "prod"
instance_type = "t3.large"
```

**Commands:**

```bash
terraform plan -var-file=dev.tfvars
terraform apply -var-file=dev.tfvars
```

For production, we should use a separate state path or backend configuration and appropriate permissions.

## 22. What are workspaces in Terraform and when would you use them?

**Answer:**

Terraform workspaces allow us to manage multiple state instances using the same Terraform configuration.

For example, we can create DEV, SIT and PROD workspaces.

**Create workspaces:**

```bash
terraform workspace new dev
terraform workspace new sit
terraform workspace new prod
```

**Select a workspace:**

```bash
terraform workspace select dev
```

**Check the current workspace:**

```bash
terraform workspace show
```

**Use the workspace name in Terraform:**

```hcl
tags = {
  Environment = terraform.workspace
}
```

**Advantages:**

- Same code.
- Separate state for each workspace.
- Less code duplication.

**Limitations:**

- Workspaces do not automatically provide separate AWS account isolation.
- Selecting the wrong workspace can cause accidental changes.
- A code change can affect multiple workspaces.

## 23. How do you handle sensitive data like passwords in Terraform?

**Answer:**

We should not hardcode passwords, secret keys or database credentials directly in Terraform files.

We can use services such as AWS Secrets Manager or AWS Systems Manager Parameter Store.

We can also mark variables as sensitive.

**Example:**

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

The `sensitive` argument hides values in normal CLI output in supported contexts, but it does not automatically remove the value from Terraform state.

Therefore, we must secure state storage with encryption, restricted IAM access and appropriate secret-management practices.

## 24. What is the difference between terraform taint and terraform replace?

**Answer:**

Both are related to replacing a resource.

Earlier, Terraform used the taint command to mark a resource for replacement.

```bash
terraform taint aws_instance.mongodb
```

The `terraform taint` command is deprecated.

The recommended approach is:

```bash
terraform plan -replace=aws_instance.mongodb
```

To execute the replacement:

```bash
terraform apply -replace=aws_instance.mongodb
```

**Difference:**

- `taint` is the older approach.
- `-replace` is the modern recommended approach.

## 25. How do you use lifecycle blocks in Terraform?

**Answer:**

Lifecycle blocks help control how Terraform manages resource creation, updates, replacement and destruction.

Three important lifecycle arguments are:

### 1. prevent_destroy

Prevents Terraform from destroying the resource while this rule remains in the configuration.

```hcl
lifecycle {
  prevent_destroy = true
}
```

### 2. create_before_destroy

Creates the replacement resource before destroying the existing resource, when supported by the resource and dependencies.

```hcl
lifecycle {
  create_before_destroy = true
}
```

### 3. ignore_changes

Ignores changes to selected attributes during normal update planning.

```hcl
lifecycle {
  ignore_changes = [
    tags
  ]
}
```

**Real-time example:**

If another automation tool manages certain EC2 tags, we can use `ignore_changes` for those tags to avoid Terraform continuously trying to overwrite them.

---

# Troubleshooting and Real-Time Scenarios

## 26. How do you debug and troubleshoot Terraform errors?

**Answer:**

Whenever I get an error while executing Terraform, I first check the error message and identify which resource or configuration is causing the issue.

**My troubleshooting steps:**

1. Check Terraform syntax.
2. Run `terraform fmt`.
3. Run `terraform validate`.
4. Run `terraform plan`.
5. Check provider configuration and AWS credentials.
6. Check resource dependencies and variable values.
7. Check state if the problem is related to state management.

**Useful commands:**

```bash
terraform fmt
terraform validate
terraform plan
terraform providers
terraform state list
```

**For detailed debugging:**

```bash
export TF_LOG=DEBUG
terraform plan
```

**To save debug logs:**

```bash
export TF_LOG_PATH=terraform.log
```

**Example:**

If Terraform fails because of an invalid AMI ID, I check whether the AMI exists in the selected AWS region, verify the ID and provider configuration, correct the issue and run plan again.

## 27. What are Provisioners in Terraform? When would you use them and what are their limitations?

**Answer:**

Provisioners execute commands or scripts during resource creation or destruction.

Three important provisioners are:

1. `local-exec` – Runs commands on the machine where Terraform is running.
2. `remote-exec` – Runs commands inside the remote server, usually through SSH.
3. `file` – Copies files from the local machine to a remote server.

**local-exec example:**

```hcl
provisioner "local-exec" {
  command = "echo EC2 instance created"
}
```

This command runs on my local machine.

**Limitations:**

- They can fail because of connectivity or script errors.
- They are less predictable than native Terraform resources.
- Terraform cannot always track the external changes made by scripts.
- They should be used as a last resort.

Whenever possible, I prefer cloud-init, user data or Ansible for server configuration.

## 28. How do you manage dependencies between resources in Terraform?

**Answer:**

Terraform automatically identifies dependencies through resource references.

For example, suppose I want to create a Security Group and attach it to an EC2 instance.

```hcl
resource "aws_security_group" "mongodb" {
  name = "mongodb-sg"
}

resource "aws_instance" "mongodb" {
  ami           = "ami-12345678"
  instance_type = "t3.micro"

  vpc_security_group_ids = [
    aws_security_group.mongodb.id
  ]
}
```

Here, the EC2 instance depends on the Security Group.

Terraform creates the Security Group first and then creates the EC2 instance.

We can also use `depends_on` when Terraform cannot identify the dependency automatically.

```hcl
depends_on = [
  aws_security_group.mongodb
]
```

**Two types:**

- Implicit dependency – Identified automatically through references.
- Explicit dependency – Defined using `depends_on`.

## 29. What are the best practices for writing production-ready Terraform code?

**Answer:**

When writing Terraform code for production, I follow these best practices:

1. Use modules for reusable infrastructure.
2. Store state in a remote backend.
3. Enable state locking where supported.
4. Maintain code in Git.
5. Use variables instead of hardcoding values.
6. Separate DEV, QA and PROD configurations and state.
7. Store sensitive information securely.
8. Follow least-privilege IAM permissions.
9. Pin provider and module versions.
10. Review Terraform plans before applying changes.
11. Use lifecycle rules carefully.
12. Enable appropriate backups and monitoring.

**Interview answer:**

In production, I focus on modular code, remote state management, version control, environment separation, security and proper plan reviews. I also follow approval processes before applying infrastructure changes in production.

## 30. How would you design a real-world Terraform project to deploy highly available, scalable and secure infrastructure on AWS or Azure?

**Answer:**

First, I understand the application requirements, availability, security and scalability requirements.

For example, let's consider a Roboshop application hosted on AWS.

I would divide the infrastructure into different layers.

### 1. Network layer

- Create a VPC.
- Create public and private subnets across multiple Availability Zones.
- Configure Internet Gateway.
- Configure NAT Gateways where required.
- Configure route tables.

### 2. Security layer

- Create Security Groups.
- Allow only required ports.
- Configure IAM roles and policies.
- Store secrets in AWS Secrets Manager.

### 3. Application layer

- Deploy application instances in private subnets.
- Use an Application Load Balancer.
- Configure Auto Scaling to handle traffic changes.

### 4. Database layer

- Use Amazon RDS with Multi-AZ where required.
- Configure database security.
- Enable automated backups.

### 5. Terraform management

- Create reusable modules.
- Maintain separate DEV, QA and PROD configurations.
- Store state in S3.
- Enable state locking.
- Maintain code in GitHub.
- Integrate Terraform with CI/CD pipelines.

**Interview answer:**

For a production application, I would design a highly available architecture across multiple Availability Zones. I would use public and private subnets, an Application Load Balancer, Auto Scaling Groups and a managed database like RDS.

I would automate the infrastructure using reusable Terraform modules. I would maintain remote state in S3, implement proper IAM permissions and integrate Terraform with CI/CD pipelines.

My main focus would be availability, scalability, security and maintainability.

---

# Quick Revision – Important Terraform Commands

| Command | Purpose |
|---|---|
| `terraform init` | Initialize Terraform |
| `terraform fmt` | Format code |
| `terraform validate` | Validate configuration |
| `terraform plan` | Preview changes |
| `terraform apply` | Apply changes |
| `terraform destroy` | Destroy managed infrastructure |
| `terraform output` | Display outputs |
| `terraform state list` | List resources in state |
| `terraform state show` | Display resource state |
| `terraform workspace list` | List workspaces |
| `terraform workspace show` | Show current workspace |
| `terraform import` | Import existing resource into state |
| `terraform plan -refresh-only` | Preview state refresh |
| `terraform apply -replace=RESOURCE` | Replace a resource |

---

# Final Interview Tips

- Do not memorize answers word by word. Understand the concept.
- Explain the purpose before showing code.
- Use AWS examples such as EC2, VPC, Security Groups and S3.
- Explain how you use Terraform in DEV, QA and PROD.
- Be prepared to explain your Terraform project architecture.
- Practise state management, modules, loops and remote backend especially well.

**Remember:** A good interview answer is a simple explanation followed by one practical example.

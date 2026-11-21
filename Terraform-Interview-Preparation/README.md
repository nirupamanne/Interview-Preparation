# Terraform – Interview Questions and Answers

**Purpose:** Simple, interview-friendly notes for Terraform revision.

**How to use:** Understand the idea, practise the example, and explain it in your own words.

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
       terraform deploy
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

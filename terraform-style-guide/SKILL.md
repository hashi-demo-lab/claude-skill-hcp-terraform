---
name: terraform-style-guide
description: Comprehensive guide for Terraform code style, formatting, and best practices based on HashiCorp's official standards. Use when writing or reviewing Terraform configurations, formatting code, organizing files and modules, establishing team conventions, managing version control, or ensuring code quality and consistency across infrastructure projects.
---

# Terraform Style Guide

Adopting and adhering to a style guide keeps your Terraform code legible, scalable, and maintainable. This guide is based on HashiCorp's official Terraform style conventions and best practices.

## Table of Contents

- [Code Style Fundamentals](#code-style-fundamentals)
- [Code Formatting Standards](#code-formatting-standards)
- [File Organization](#file-organization)
- [Naming Conventions](#naming-conventions)
- [Resource Organization](#resource-organization)
- [Variables and Outputs](#variables-and-outputs)
- [Local Values](#local-values)
- [Provider Configuration and Aliasing](#provider-configuration-and-aliasing)
- [Dynamic Resource Creation](#dynamic-resource-creation)
- [Version Control](#version-control)
- [Workflow Standards](#workflow-standards)
- [Multi-Environment Management](#multi-environment-management)
- [State and Secrets Management](#state-and-secrets-management)
- [Testing and Policy](#testing-and-policy)

---

## Code Style Fundamentals

### Core Principles

Always follow these fundamental practices:

- **Execute `terraform fmt`** before committing code to version control
- **Execute `terraform validate`** to catch syntax and configuration errors
- **Use `#` for comments** (avoid `//` and `/* */` style comments)
- **Name resources with descriptive nouns** using underscores, excluding the resource type
- **Define dependent resources after their dependencies** for better readability
- **Include type and description for all variables**
- **Include descriptions for all outputs**
- **Use `count` and `for_each` judiciously** with clear intent

### Automation with Git Hooks

Consider using Git pre-commit hooks to automatically run `terraform fmt` and `terraform validate`:

```bash
#!/bin/bash
# .git/hooks/pre-commit

terraform fmt -recursive
terraform validate
```

---

## Code Formatting Standards

Terraform has specific formatting conventions that the `terraform fmt` command automates.

### Indentation

- Use **two spaces** per nesting level
- Never use tabs

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```

### Alignment

- **Align equals signs** for consecutive single-line arguments at the same nesting level
- Separate different argument groups with blank lines

```hcl
# Good - aligned equals signs
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = "subnet-12345678"

  tags = {
    Name        = "web-server"
    Environment = "production"
  }
}

# Bad - unaligned
resource "aws_instance" "web" {
  ami = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id = "subnet-12345678"
}
```

### Block Organization

- **Arguments precede blocks** within a resource
- **Separate with one blank line** between arguments and blocks
- **Meta-arguments come first**, followed by standard arguments, then blocks

```hcl
resource "aws_instance" "example" {
  # Meta-arguments first
  count = 3

  # Standard arguments
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  # Blocks last
  root_block_device {
    volume_size = 20
  }
}
```

### Spacing

- Use **single blank lines** to separate logical groups of arguments
- **Top-level blocks** (resources, data sources, modules) require blank lines between them
- Do not use excessive blank lines

```hcl
variable "instance_count" {
  description = "Number of instances to create"
  type        = number
  default     = 1
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}
```

---

## File Organization

### Standard File Structure

Organize your Terraform code into these standard files:

| File | Purpose |
|------|---------|
| `terraform.tf` | Terraform and provider version requirements |
| `providers.tf` | Provider configurations |
| `main.tf` | Primary resources and data sources |
| `variables.tf` | Input variable declarations (alphabetical order) |
| `outputs.tf` | Output value declarations (alphabetical order) |
| `locals.tf` | Local value declarations |

Example `terraform.tf`:

```hcl
terraform {
  required_version = ">= 1.7"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.34.0"
    }
  }
}
```

Example `providers.tf`:

```hcl
provider "aws" {
  region = var.aws_region

  default_tags {
    tags = {
      ManagedBy = "Terraform"
      Project   = "MyProject"
    }
  }
}
```


---

## Naming Conventions

### General Rules

- Use **descriptive nouns and underscores** to separate multiple words
- **Exclude the resource type** from the resource name (redundant)
- Use **lowercase** for all names
- Be **specific and meaningful**

### Examples

```hcl
# ❌ Bad - includes resource type, uses hyphens, mixed case
resource "aws_instance" "webAPI-aws-instance" {
  # ...
}

# ✅ Good - descriptive noun, underscores, lowercase
resource "aws_instance" "web_api" {
  # ...
}

# ❌ Bad - too generic
variable "name" {
  type = string
}

# ✅ Good - specific and clear
variable "application_name" {
  type = string
}
```

### Variable Naming

Variables should clearly indicate their purpose:

```hcl
variable "vpc_cidr_block" {
  description = "CIDR block for the VPC"
  type        = string
}

variable "enable_dns_hostnames" {
  description = "Enable DNS hostnames in the VPC"
  type        = bool
  default     = true
}
```

---

## Resource Organization

### Dependency Order

**Define a data source before the resource that references it** for better readability:

```hcl
# Data source first
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

# Resource that uses it second
resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t2.micro"
}
```

### Parameter Order Within Resources

Follow this standard ordering for resource parameters:

1. **`count` or `for_each`** (meta-arguments)
2. **Resource-specific non-block parameters** (alphabetically or logically grouped)
3. **Resource-specific block parameters**
4. **`lifecycle` block** (if needed)
5. **`depends_on`** (if required, as last resort)

```hcl
resource "aws_instance" "web" {
  # 1. Meta-arguments
  count = var.instance_count

  # 2. Non-block parameters
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.public.id

  # 3. Block parameters
  root_block_device {
    volume_size = 20
    volume_type = "gp3"
  }

  tags = {
    Name = "web-${count.index}"
  }

  # 4. Lifecycle
  lifecycle {
    create_before_destroy = true
  }

  # 5. depends_on (avoid if possible)
  # depends_on = [aws_iam_role_policy.example]
}
```

---

## Variables and Outputs

### Variable Declaration Standards

**Every variable must include:**
- `type` - the data type
- `description` - clear explanation of purpose

**Optional but recommended:**
- `default` - default value if applicable
- `sensitive` - mark as true for secrets
- `validation` - for uniquely restrictive requirements

```hcl
variable "instance_type" {
  description = "EC2 instance type for the web server"
  type        = string
  default     = "t2.micro"

  validation {
    condition     = contains(["t2.micro", "t2.small", "t2.medium"], var.instance_type)
    error_message = "Instance type must be t2.micro, t2.small, or t2.medium."
  }
}

variable "database_password" {
  description = "Password for the database admin user"
  type        = string
  sensitive   = true
}

variable "availability_zones" {
  description = "List of availability zones for resource placement"
  type        = list(string)
}

variable "tags" {
  description = "Common tags to apply to all resources"
  type        = map(string)
  default     = {}
}
```

### Output Declaration Standards

**Every output must include:**
- `description` - clear explanation of the value

**Optional attributes:**
- `sensitive` - mark as true to hide from console output
- `depends_on` - explicit dependencies if needed

```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.web.public_ip
}

output "database_password" {
  description = "Database administrator password"
  value       = aws_db_instance.main.password
  sensitive   = true
}
```

### Variable Files Organization

Organize variables alphabetically in `variables.tf` and use `.tfvars` files for environment-specific values:

```hcl
# terraform.tfvars (or dev.tfvars, prod.tfvars)
instance_type      = "t2.micro"
instance_count     = 3
availability_zones = ["us-west-2a", "us-west-2b"]
```

---

## Local Values

### Usage Guidelines

Use local values **sparingly** to avoid unnecessary complexity. Locals are appropriate when:
- Avoiding repetition of complex expressions
- Giving meaningful names to intermediate values
- Computing values used multiple times

```hcl
locals {
  # Good use case - computing a reusable value
  common_tags = merge(
    var.tags,
    {
      Environment = var.environment
      ManagedBy   = "Terraform"
      Project     = var.project_name
    }
  )

  # Good use case - naming a complex expression
  vpc_id = var.create_vpc ? aws_vpc.main[0].id : data.aws_vpc.existing[0].id
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type

  tags = local.common_tags
}
```

### Anti-patterns to Avoid

```hcl
# ❌ Bad - unnecessary local for a simple reference
locals {
  instance_type = var.instance_type
}

# ✅ Good - use the variable directly
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

---

## Provider Configuration and Aliasing

### Default Provider First

**Always define a default provider configuration first**, then aliases:

```hcl
# Default provider
provider "aws" {
  region = "us-west-2"
}

# Aliased provider for another region
provider "aws" {
  alias  = "east"
  region = "us-east-1"
}

# Using the aliased provider
resource "aws_instance" "east_web" {
  provider = aws.east

  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### Module Provider Configuration

For modules that use multiple providers, specify via the `providers` meta-argument:

```hcl
module "vpc_replication" {
  source = "./modules/vpc"

  providers = {
    aws.primary   = aws
    aws.secondary = aws.east
  }
}
```

---

## Dynamic Resource Creation

### count vs for_each

Choose the appropriate meta-argument based on your use case:

**Use `for_each`** when:
- Resources need distinct argument values
- You want to reference resources by key instead of index
- Resources are based on a map or set
- preference for_each over count

**Use `count`** when:
- Conditional resource creation (0 or 1)
- Do no use count for simple numeric repetition

### for_each Examples

```hcl
# Using for_each with a map
variable "instances" {
  type = map(object({
    instance_type = string
    ami           = string
  }))
  default = {
    web = {
      instance_type = "t2.micro"
      ami           = "ami-0c55b159cbfafe1f0"
    }
    api = {
      instance_type = "t2.small"
      ami           = "ami-0c55b159cbfafe1f0"
    }
  }
}

resource "aws_instance" "servers" {
  for_each = var.instances

  ami           = each.value.ami
  instance_type = each.value.instance_type

  tags = {
    Name = each.key
  }
}

# Reference: aws_instance.servers["web"].id
```

```hcl
# Using for_each with a set
variable "subnet_cidrs" {
  type    = set(string)
  default = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
}

resource "aws_subnet" "private" {
  for_each = var.subnet_cidrs

  vpc_id     = aws_vpc.main.id
  cidr_block = each.value

  tags = {
    Name = "private-${each.key}"
  }
}
```

### count Examples

```hcl
# Conditional resource creation
variable "enable_monitoring" {
  type    = bool
  default = false
}

resource "aws_cloudwatch_metric_alarm" "cpu" {
  count = var.enable_monitoring ? 1 : 0

  alarm_name          = "high-cpu-usage"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 2
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
}

# Reference (when created): aws_cloudwatch_metric_alarm.cpu[0].id
```
## Avoid the below example
```hcl
# Simple numeric repetition
variable "instance_count" {
  type    = number
  default = 3
}

resource "aws_instance" "web" {
  count = var.instance_count

  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "web-${count.index}"
  }
}

# Reference: aws_instance.web[0].id, aws_instance.web[1].id, etc.
```

---

## Version Control

### .gitignore Configuration

**Never commit to version control:**
- State files (`terraform.tfstate`, `terraform.tfstate.backup`)
- Lock info files (`.terraform.tfstate.lock.info`)
- `.terraform` directory (provider plugins and modules)
- Saved plan files (`*.tfplan`, `plan.out`)
- `.tfvars` files containing sensitive data

**Always commit:**
- All `.tf` configuration files
- `.terraform.lock.hcl` (dependency lock file)
- `.gitignore` file
- README and documentation files

---

## Workflow Standards

### Version Pinning

**Always pin versions explicitly** to ensure reproducible deployments:

```hcl
terraform {
  required_version = ">= 1.7"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.34.0"  # Pin to exact version for stability
    }
    random = {
      source  = "hashicorp/random"
      version = "~> 3.6"  # Allow patch updates only
    }
  }
}
```

**Version constraint operators:**
- `= 1.0.0` - Exact version only
- `>= 1.0.0` - Greater than or equal to
- `~> 1.0` - Allow rightmost version component to increment (1.0, 1.1, but not 2.0)
- `>= 1.0, < 2.0` - Version range

### Module Repository Naming

Use the convention: `terraform-<PROVIDER>-<NAME>`

Examples:
- `terraform-aws-vpc`
- `terraform-azurerm-virtual-network`
- `terraform-google-kubernetes-engine`

### Repository Strategy

Three common approaches:

**1. Separate Module Repositories** (Recommended)
- Each module in its own repository
- Independent versioning and release cycles
- Clear ownership boundaries

**2. Logical Infrastructure Grouping**
- Group related resources per repository
- Example: `infra-networking`, `infra-compute`, `infra-databases`
- Easier to manage related changes together

**3. Monorepo**
- All infrastructure code in one repository
- Centralized management
- Requires careful CI/CD targeting

### Branching Strategy

Adopt **GitHub Flow** for simplicity:

1. Create **short-lived feature branches** from main
2. Submit **pull requests** for review
3. Enable **speculative plans** in HCP Terraform (automatic on PRs)
4. **Merge** to main after approval
5. **Delete** feature branches after merge

```bash
# Create feature branch
git checkout -b feature/add-monitoring

# Make changes and commit
git add .
git commit -m "Add CloudWatch monitoring for EC2 instances"

# Push and create PR
git push origin feature/add-monitoring
```

---

## Multi-Environment Management

### Workspace-Based Approach (Recommended with HCP Terraform)

Use separate workspaces for each environment:

```hcl
# Development workspace: app-dev
# Staging workspace: app-staging
# Production workspace: app-prod
```

Terraform Cloud/HCP Terraform automatically manages state per workspace.

---

## State and Secrets Management

### Use HCP Terraform for state storage

### State File Security

**Never share full state files directly**. State files contain sensitive information.

**Alternatives for sharing data:**

1. **tfe_outputs data source** (HCP Terraform):

```hcl
data "tfe_outputs" "vpc" {
  organization = "my-org"
  workspace    = "networking-prod"
}

resource "aws_instance" "web" {
  subnet_id = data.tfe_outputs.vpc.values.private_subnet_ids[0]
}
```

2. **Provider-specific data sources**:

```hcl
data "aws_vpc" "main" {
  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "app" {
  vpc_id = data.aws_vpc.main.id
}
```

### Secrets Management

**Protect credentials** through:

1. **Dynamic Provider Credentials** (HCP Terraform)
   - OIDC-based authentication
   - No long-lived credentials in configuration

2. **HashiCorp Vault Integration**:

```hcl
data "vault_generic_secret" "database" {
  path = "secret/database"
}

resource "aws_db_instance" "main" {
  username = data.vault_generic_secret.database.data["username"]
  password = data.vault_generic_secret.database.data["password"]
}
```

3. **Environment Variables**:

```hcl
variable "database_password" {
  description = "Database password (set via TF_VAR_database_password)"
  type        = string
  sensitive   = true
}
```

```bash
export TF_VAR_database_password="secure-password"
terraform apply
```

---

## Testing

### Module Testing

Write tests for modules using **Terraform's native testing framework**:

```hcl
# tests/vpc.tftest.hcl
run "valid_vpc_cidr" {
  command = plan

  variables {
    vpc_cidr = "10.0.0.0/16"
  }

  assert {
    condition     = aws_vpc.main.cidr_block == "10.0.0.0/16"
    error_message = "VPC CIDR block did not match expected value"
  }
}

run "vpc_enables_dns" {
  command = plan

  assert {
    condition     = aws_vpc.main.enable_dns_hostnames == true
    error_message = "VPC should have DNS hostnames enabled"
  }
}
```

Run tests:

```bash
terraform test
```


### Common Testing Scenarios

1. **Validation Tests** - Verify variable constraints
2. **Plan Tests** - Check expected resources will be created
3. **Apply Tests** - Test actual resource creation (in isolated environment)
4. **Integration Tests** - Verify resources work together correctly

---

## Summary Checklist

Use this checklist for code reviews:

- [ ] Code formatted with `terraform fmt`
- [ ] Configuration validated with `terraform validate`
- [ ] Files organized according to standard structure
- [ ] All variables have type and description
- [ ] All outputs have descriptions
- [ ] Resource names use descriptive nouns with underscores
- [ ] Resources ordered with dependencies first
- [ ] Version constraints pinned explicitly
- [ ] Sensitive values marked with `sensitive = true`
- [ ] `.gitignore` excludes state files and secrets
- [ ] Tests written for modules
- [ ] Policy requirements satisfied
- [ ] Code reviewed by teammate

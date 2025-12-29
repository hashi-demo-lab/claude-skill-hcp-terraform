# Terraform LSP Plugin

Language Server Protocol (LSP) integration for HashiCorp Terraform using [terraform-ls](https://github.com/hashicorp/terraform-ls), the official Terraform language server.

## IDE Integration Only

This plugin provides `lspServers` configuration for Claude Code IDE extensions:

- **VS Code** - Works with the Claude Code VS Code extension
- **JetBrains** - Works with the Claude Code JetBrains plugin

**Note:** The Claude Code CLI's LSP tool does not currently consume plugin-defined language servers. This plugin only enhances IDE-based Claude Code experiences.

## Features

When enabled in a supported IDE, this plugin provides enhanced code intelligence for Terraform files:

- **Completion** - Auto-complete for resources, attributes, and variables
- **Diagnostics** - Real-time syntax and validation errors
- **Hover** - Documentation on hover for resources and attributes
- **Go to Definition** - Navigate to variable and module definitions
- **Document Symbols** - Outline view of resources and variables
- **Formatting** - Format documents using `terraform fmt`

## Prerequisites

You must have `terraform-ls` installed on your system before using this plugin.

### Installation

```bash
# Using Go
go install github.com/hashicorp/terraform-ls@latest
```

#### On MacOS

```bash
brew install hashicorp/tap/terraform-ls
```

**Manual Installation:**
1. Download the appropriate binary from [HashiCorp Releases](https://releases.hashicorp.com/terraform-ls/)
2. Extract and move to a directory in your `PATH`
3. Verify installation: `terraform-ls -v`

## Supported File Types

| Extension | Language |
|-----------|----------|
| `.tf` | Terraform configuration |
| `.tfvars` | Terraform variables |

## Usage

Once terraform-ls is installed and you have the Claude Code IDE extension (VS Code or JetBrains), the language server activates automatically when you open `.tf` or `.tfvars` files.

## Resources

- [terraform-ls GitHub Repository](https://github.com/hashicorp/terraform-ls)
- [terraform-ls Documentation](https://github.com/hashicorp/terraform-ls/tree/main/docs)
- [HashiCorp Terraform](https://www.terraform.io/)

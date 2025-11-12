# HCP Terraform Skills Marketplace

A Claude Code marketplace providing specialized skills for HashiCorp Terraform development. Install these skills to gain expert guidance on Terraform style, testing, Stacks configurations, and HCP Terraform automation.

## Available Skills

### 🎨 Terraform Style Guide

HashiCorp's official Terraform style conventions and Azure Verified Modules (AVM) requirements. Covers formatting, naming, file organization, and best practices.

### 🧪 Terraform Test

Complete guide for Terraform's native testing framework. Learn to write unit and integration tests using `.tftest.hcl` files with run blocks, assertions, and mock providers.

### 📚 Terraform Stacks

Comprehensive guide for HCP Terraform Stacks. Build multi-region, multi-environment deployments with components, deployments, and OIDC authentication.

### 🤖 Terraform MCP as Code

Automate Terraform Cloud/Enterprise operations programmatically. Includes TypeScript wrappers for creating workspaces, triggering runs, managing variables, and searching registries via the HashiCorp MCP server.

# Installation of HCP Terraform - Claude Skills

## Add the marketplace
```bash
/plugin marketplace add hashi-demo-lab/claude-skill-hcp-terraform
```

## Install the plugin (includes all three skills)
```bash
/plugin install hcp-terraform-skills
```

This installs all four skills:
- `terraform-style-guide` - HashiCorp Terraform style conventions
- `terraform-test` - Terraform testing framework guidance
- `terraform-stacks` - HCP Terraform Stacks configurations
- `terraform-mcp-as-code` - HCP Terraform automation via MCP server

### Team Installation

Add to `.claude/settings.json` for automatic marketplace access:

```json
{
  "extraKnownMarketplaces": [
    {
      "source": "github",
      "repo": "hashi-demo-lab/claude-skill-hcp-terraform"
    }
  ]
}
```

## Usage

Skills activate automatically when working with Terraform files:

- **`.tf` files** → Style guide applied
- **`.tftest.hcl` files** → Test guidance provided
- **`.tfcomponent.hcl`, `.tfdeploy.hcl` files** → Stacks guidance provided
- **MCP automation** → Invoke explicitly for programmatic HCP Terraform operations

## Repository Structure

```text
claude-skill-hcp-terraform/
├── .claude-plugin/
│   └── marketplace.json              # Marketplace catalog
├── terraform-style-guide/
│   ├── plugin.json                   # Skill metadata
│   ├── SKILL.md                      # Main skill content
│   ├── CLAUDE.md                     # Claude Code guidance
│   ├── README.md                     # Skill documentation
│   └── AVM.md                        # Azure Verified Modules reference
├── terraform-test/
│   ├── plugin.json                   # Skill metadata
│   ├── SKILL.md                      # Main skill content
│   ├── CLAUDE.md                     # Claude Code guidance
│   └── README.md                     # Skill documentation
├── terraform-stacks/
│   ├── plugin.json                   # Skill metadata
│   ├── SKILL.md                      # Main skill content
│   ├── CLAUDE.md                     # Claude Code guidance
│   ├── README.md                     # Skill documentation
│   └── references/                   # Additional documentation
│       ├── component-blocks.md
│       ├── deployment-blocks.md
│       └── examples.md
└── terraform-mcp-as-code/
    ├── plugin.json                   # Skill metadata
    ├── SKILL.md                      # Main skill content
    ├── CLAUDE.md                     # Claude Code guidance
    ├── README.md                     # Skill documentation
    └── scripts/                      # TypeScript wrapper functions
        ├── client.ts
        ├── organization/
        ├── private-registry/
        ├── public-registry/
        ├── runs/
        ├── variables/
        └── workspaces/
```

## Requirements

- **Terraform Style Guide**: Terraform 1.0+
- **Terraform Test**: Terraform 1.6.0+ (mock providers require 1.7.0+)
- **Terraform Stacks**: HCP Terraform or Terraform Enterprise
- **Terraform MCP as Code**: Docker, Terraform Cloud/Enterprise account with API token

## Contributing

Contributions welcome! Open issues or submit pull requests for:

- Documentation improvements
- New examples and patterns
- Bug fixes and clarifications

When contributing:

1. Test all code examples
2. Maintain consistent terminology
3. Follow existing documentation structure

## Resources

- [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)
- [Terraform Testing](https://developer.hashicorp.com/terraform/language/tests)
- [HCP Terraform Stacks](https://developer.hashicorp.com/terraform/cloud-docs/stacks)
- [Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/)

## License

MIT License - see [LICENSE](LICENSE) file for details.

---

**Note**: These are Claude Code skills (knowledge bases), not executable code. They provide expertise to Claude when assisting with Terraform development.

Skills are model-invoked—Claude automatically uses them when relevant to your task.

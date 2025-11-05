# HCP Terraform Skills Marketplace

A Claude Code marketplace providing specialized skills for working with HashiCorp Terraform, Terraform Stacks, and infrastructure testing. This marketplace enables teams to discover, install, and use comprehensive Terraform expertise directly within Claude Code.

## Available Skills

### 🎨 Terraform Style Guide
**Directory:** `terraform-style-guide/`

Comprehensive guide for Terraform code style, formatting, and best practices based on HashiCorp's official standards and Azure Verified Modules (AVM) requirements.

**Use when:**
- Writing or reviewing Terraform configurations
- Formatting code before commits
- Establishing team coding conventions
- Organizing files and modules
- Ensuring code quality and consistency
- Developing Azure Verified Modules

**Key Features:**
- Core formatting standards (indentation, alignment, spacing)
- Naming conventions and file organization
- Variables, outputs, and local values best practices
- Dynamic resource creation patterns (`for_each` vs `count`)
- Version control and workflow standards
- Azure Verified Modules (AVM) requirements
- Multi-environment management
- Testing and policy enforcement

### 🧪 Terraform Test
**Directory:** `terraform-test/`

Complete guide for writing and running automated tests for Terraform configurations using Terraform's native testing framework.

**Use when:**
- Creating test files (`.tftest.hcl`)
- Writing test scenarios with run blocks
- Validating infrastructure behavior with assertions
- Mocking providers and data sources
- Testing module outputs and resource configurations
- Setting up CI/CD test pipelines
- Troubleshooting Terraform test syntax

**Key Features:**
- Test file structure and components
- Integration tests (`command = apply`) vs unit tests (`command = plan`)
- Assertion blocks and expect_failures
- Mock providers for isolated testing (Terraform 1.7.0+)
- Test execution commands and options
- Sequential and parallel test execution
- CI/CD integration patterns
- Common testing scenarios and troubleshooting

### 📚 Terraform Stacks
**Directory:** `terraform-stacks/`

Comprehensive guide for working with HashiCorp Terraform Stacks, enabling infrastructure deployments across multiple environments, regions, and accounts.

**Use when:**
- Creating Stack configurations (`.tfcomponent.hcl`, `.tfdeploy.hcl`)
- Managing multi-region deployments
- Working with multi-environment infrastructure (dev/staging/prod)
- Configuring cross-stack dependencies
- Setting up OIDC authentication for providers
- Working with HCP Terraform
- Troubleshooting Terraform Stacks syntax

**Key Features:**
- Component and deployment configuration syntax
- Multi-region and multi-environment patterns
- Provider configurations with `for_each`
- Linked Stacks with upstream/downstream dependencies
- OIDC identity token configuration
- Deployment groups and auto-approval rules
- Complete working examples from simple to complex
- Private and public registry module sources

## Installation

### Prerequisites

- **Claude Code** installed and configured
- Git (for cloning this repository)

### Quick Install (Recommended)

Install the entire marketplace with all three skills:

#### Option 1: Install from GitHub

```bash
# In Claude Code
/plugin marketplace add hashi-demo-lab/claude-skill-hcp-terraform
```

Then install individual skills:

```bash
/plugin install terraform-style-guide@hcp-terraform-skills
/plugin install terraform-test@hcp-terraform-skills
/plugin install terraform-stacks@hcp-terraform-skills
```

#### Option 2: Install from Local Clone

```bash
# Clone the repository
git clone https://github.com/hashi-demo-lab/claude-skill-hcp-terraform.git

# In Claude Code, add the local marketplace
/plugin marketplace add file:///path/to/claude-skill-hcp-terraform

# Install individual skills
/plugin install terraform-style-guide@hcp-terraform-skills
/plugin install terraform-test@hcp-terraform-skills
/plugin install terraform-stacks@hcp-terraform-skills
```

### Team Installation

For team-wide access, configure automatic marketplace installation in `.claude/settings.json`:

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

Team members can then install skills directly:

```bash
/plugin install terraform-style-guide@hcp-terraform-skills
```

### Alternative: Direct Skill Installation

Install skills individually without the marketplace:

```bash
# Install specific skills directly from GitHub subdirectories
/plugin install github:hashi-demo-lab/claude-skill-hcp-terraform/terraform-style-guide
/plugin install github:hashi-demo-lab/claude-skill-hcp-terraform/terraform-test
/plugin install github:hashi-demo-lab/claude-skill-hcp-terraform/terraform-stacks
```

### Verify Installation

List installed marketplaces:

```bash
/plugin marketplace list
```

List available plugins:

```bash
/plugin list
```

## Usage

### Using Skills in Claude Code

Once installed, these skills are automatically available when working with Terraform files. Claude will proactively reference them when:

- You create or edit `.tf` files (style guide is applied)
- You create or edit `.tftest.hcl` files (test guidance is provided)
- You create or edit `.tfcomponent.hcl` or `.tfdeploy.hcl` files (stacks guidance is provided)

### Explicit Skill Invocation

You can explicitly invoke a skill by mentioning it in your request:

```
"Review this Terraform code using the style guide"
"Help me write tests for this module"
"Create a multi-region stack configuration"
```

### Example Workflows

#### 1. Writing Clean Terraform Code

```
You: "Help me refactor this Terraform module following best practices"

Claude: [Uses terraform-style-guide skill]
- Applies formatting standards
- Suggests proper naming conventions
- Recommends file organization
- Identifies anti-patterns
```

#### 2. Creating Tests

```
You: "Create tests for my VPC module"

Claude: [Uses terraform-test skill]
- Creates tests/ directory structure
- Writes unit tests with command = plan
- Adds integration tests with command = apply
- Includes assertions for key outputs
```

#### 3. Building Multi-Environment Stacks

```
You: "Set up a Stack with dev, staging, and prod environments"

Claude: [Uses terraform-stacks skill]
- Creates component configurations
- Sets up deployment blocks for each environment
- Configures provider settings
- Establishes deployment groups
```

## Repository Structure

This marketplace repository follows the Claude Code plugin marketplace schema:

```
claude-skill-hcp-terraform/
├── marketplace.json                  # Marketplace catalog (lists all plugins)
├── README.md                         # This file
├── terraform-style-guide/            # Plugin: Terraform Style Guide
│   ├── plugin.json                  # Plugin metadata
│   ├── SKILL.md                     # Skill content (main)
│   ├── CLAUDE.md                    # Claude guidance
│   ├── AVM.md                       # AVM requirements reference
│   └── README.md                    # Brief description
├── terraform-test/                   # Plugin: Terraform Test
│   ├── plugin.json                  # Plugin metadata
│   ├── SKILL.md                     # Skill content (main)
│   ├── CLAUDE.md                    # Claude guidance
│   └── README.md                    # Brief description
└── terraform-stacks/                 # Plugin: Terraform Stacks
    ├── plugin.json                  # Plugin metadata
    ├── SKILL.md                     # Skill content (main)
    ├── CLAUDE.md                    # Claude guidance
    ├── README.md                    # Brief description
    └── references/                  # Additional documentation
        ├── component-blocks.md      # Component syntax reference
        ├── deployment-blocks.md     # Deployment syntax reference
        └── examples.md              # Complete examples
```

### Key Files

**Marketplace Level:**
- **marketplace.json**: Catalog file that lists all available plugins with metadata, sources, and keywords

**Plugin Level:**
- **plugin.json**: Plugin metadata including name, version, author, and skills list
- **SKILL.md**: The main skill content that Claude references (required)
- **CLAUDE.md**: Internal guidance for Claude on how to interpret and apply the skill
- **README.md**: Brief description for humans browsing the repository
- **references/**: Additional reference documentation (for complex skills)

## Requirements

### Terraform Versions

- **Terraform Style Guide**: Compatible with Terraform 1.0+
- **Terraform Test**: Requires Terraform 1.6.0+ (mock providers require 1.7.0+)
- **Terraform Stacks**: Requires HCP Terraform or Terraform Enterprise

### HCP Terraform

The Terraform Stacks skill requires an HCP Terraform account:
- Free tier available at [app.terraform.io](https://app.terraform.io)
- Terraform Enterprise for self-hosted deployments

## Contributing

Contributions are welcome! If you find errors, have suggestions, or want to add new patterns:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Make your changes
4. Commit with clear messages (`git commit -m "Add example for X"`)
5. Push to your fork (`git push origin feature/improvement`)
6. Open a Pull Request

### Documentation Style

When contributing:

- Use clear, concise language
- Include working code examples
- Test all HCL syntax examples
- Maintain consistency with existing terminology
- Update relevant examples when changing syntax
- Follow the same structure as existing skills

### Adding New Skills

To add a new skill to the marketplace:

1. Create a new directory for the skill: `mkdir new-skill/`
2. Create `plugin.json` with metadata:
   ```json
   {
     "name": "new-skill",
     "description": "Brief description",
     "version": "1.0.0",
     "author": "Your Name",
     "skills": [{"name": "new-skill", "path": "SKILL.md"}]
   }
   ```
3. Create `SKILL.md` with YAML frontmatter:
   ```markdown
   ---
   name: new-skill
   description: When to use this skill
   ---
   # Skill Content
   ```
4. Add entry to `marketplace.json` plugins array:
   ```json
   {
     "name": "new-skill",
     "description": "Brief description",
     "version": "1.0.0",
     "source": "./new-skill"
   }
   ```
5. Test installation locally before submitting PR

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Support and Resources

### Official Documentation

- [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)
- [Terraform Testing Documentation](https://developer.hashicorp.com/terraform/language/tests)
- [HCP Terraform Stacks](https://developer.hashicorp.com/terraform/cloud-docs/stacks)
- [Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/)

### Community

- [HashiCorp Discuss](https://discuss.hashicorp.com/c/terraform-core)
- [Terraform GitHub Issues](https://github.com/hashicorp/terraform/issues)
- [Azure Verified Modules GitHub](https://github.com/Azure/terraform-azurerm-avm)

### Getting Help

If you encounter issues with these skills:

1. Check the SKILL.md file in the relevant skill directory
2. Review the examples in `references/examples.md` (for Terraform Stacks)
3. Consult the official HashiCorp documentation
4. Open an issue in this repository

## Updates and Maintenance

These skills are maintained to reflect:
- Latest Terraform features and best practices
- HashiCorp official style conventions
- Azure Verified Modules requirements
- Community feedback and real-world usage patterns

Check the commit history for recent updates and changes.

## Version History

### Current Version: 1.0.0

**Terraform Style Guide:**
- Complete HashiCorp style conventions
- Azure Verified Modules (AVM) requirements
- Best practices for module development

**Terraform Test:**
- Native Terraform testing framework (1.6.0+)
- Mock providers support (1.7.0+)
- Integration and unit testing patterns

**Terraform Stacks:**
- Component and deployment configurations
- Multi-region and multi-environment patterns
- Linked Stacks and OIDC authentication

---

## About This Marketplace

This is a **Claude Code marketplace** containing skills (not executable code libraries). Skills are knowledge bases that provide documentation, best practices, and guidance to Claude Code when assisting with Terraform development.

When you install these skills, Claude gains expertise in:
- HashiCorp Terraform formatting and style conventions
- Azure Verified Modules (AVM) requirements
- Terraform testing frameworks and patterns
- HCP Terraform Stacks configurations
- Multi-region and multi-environment deployments

Skills are model-invoked—Claude automatically uses them when relevant to your task, requiring no explicit commands from you.

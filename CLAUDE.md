# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code Marketplace repository** containing specialized skills for HashiCorp Terraform development. This repository is NOT a traditional software project with executable code—it's a collection of documentation-based skills (knowledge bases) that provide expertise to Claude Code when assisting with Terraform development.

**Purpose**: Provide comprehensive guidance on Terraform style conventions, testing frameworks, Stacks configurations, and programmatic HCP Terraform automation to Claude Code users working with Terraform infrastructure-as-code.

## Repository Architecture

### Marketplace Structure

The repository follows the Claude Code marketplace plugin structure:

```
claude-skill-hcp-terraform/
├── .claude-plugin/
│   └── marketplace.json              # Marketplace catalog definition
├── terraform-style-guide/            # Skill 1: Style guide
│   ├── plugin.json                   # Skill metadata
│   ├── SKILL.md                      # Main skill content
│   ├── CLAUDE.md                     # Claude-specific guidance
│   ├── README.md                     # User-facing documentation
│   └── AVM.md                        # Azure Verified Modules reference
├── terraform-test/                   # Skill 2: Testing framework
│   ├── plugin.json
│   ├── SKILL.md
│   ├── CLAUDE.md
│   └── README.md
├── terraform-stacks/                 # Skill 3: Stacks configurations
│   ├── plugin.json
│   ├── SKILL.md
│   ├── CLAUDE.md
│   ├── README.md
│   └── references/                   # Additional reference docs
│       ├── component-blocks.md
│       ├── deployment-blocks.md
│       └── examples.md
└── terraform-mcp-as-code/            # Skill 4: MCP-based automation
    ├── SKILL.md
    ├── README.md
    └── scripts/                      # TypeScript wrapper functions
        ├── client.ts
        ├── organization/
        ├── private-registry/
        ├── public-registry/
        ├── runs/
        ├── variables/
        └── workspaces/
```

### Installation Commands

Users install this marketplace and its skills using:

```bash
# Add the marketplace
/plugin marketplace add hashi-demo-lab/claude-skill-hcp-terraform

# Install all skills
/plugin install hcp-terraform-skills
```

For team installation, users add to `.claude/settings.json`:

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

## Skill Activation Patterns

Each skill activates automatically when Claude Code detects specific file patterns:

- **terraform-style-guide**: Activates for `.tf` files
- **terraform-test**: Activates for `.tftest.hcl` files
- **terraform-stacks**: Activates for `.tfcomponent.hcl` and `.tfdeploy.hcl` files
- **terraform-mcp-as-code**: Invoke explicitly for programmatic HCP Terraform operations

## Critical Architecture Concepts

### Skills Are Documentation, Not Code

This repository contains **knowledge bases**, not executable software:
- No build system, compilation, or package management
- No test suites or CI/CD pipelines
- All "code examples" in SKILL.md files are documentation, not executed
- Changes require documentation updates, not code refactoring

### File Organization Pattern

Each skill follows a consistent structure:

1. **plugin.json**: Metadata (name, description, version, keywords, skill path)
2. **SKILL.md**: Comprehensive technical content consumed by Claude Code
3. **CLAUDE.md**: Meta-guidance for Claude Code on how to use the skill
4. **README.md**: User-facing documentation (brief, installation-focused)

### Marketplace Configuration

The `.claude-plugin/marketplace.json` defines:
- Marketplace metadata (name, owner, description, version)
- Plugin collection with skill paths
- `strict: false` allows flexible skill loading

## Content Organization Philosophy

### SKILL.md Files

These are the primary knowledge source consumed by Claude Code:
- Comprehensive technical reference
- Syntax specifications
- Code examples
- Best practices
- Troubleshooting guides

**Critical**: All code examples must use correct, valid syntax for the respective technology (Terraform, Terraform Test, Terraform Stacks HCL).

### CLAUDE.md Files

These provide **meta-guidance** to Claude Code:
- How to interpret the skill content
- Common user scenarios and questions
- Key concepts to emphasize
- Anti-patterns to avoid
- Quick reference guides

**Critical**: These files teach Claude Code how to be an effective Terraform assistant, not just what Terraform is.

### README.md Files

User-facing documentation:
- Brief skill description
- Installation instructions
- Usage patterns
- Requirements
- External resource links

**Critical**: Keep these concise—they're for human users browsing the marketplace, not for Claude Code consumption.

## Individual Skill Architectures

### terraform-style-guide

**Content**: HashiCorp's official Terraform style conventions + Azure Verified Modules (AVM) requirements

**Key Files**:
- SKILL.md: Complete style guide (formatting, naming, file organization, version control)
- AVM.md: Azure-specific requirements and patterns
- CLAUDE.md: Guidance on proactive style enforcement

**When to reference**: Any `.tf` file work—writing, reviewing, or refactoring Terraform configurations.

### terraform-test

**Content**: Terraform's native testing framework (`.tftest.hcl`)

**Key Concepts**:
- Test modes: `command = apply` (integration) vs `command = plan` (unit)
- Run blocks execute sequentially by default
- Mock providers for isolated testing (Terraform 1.7.0+)
- Test organization: separate unit tests from integration tests

**When to reference**: Writing tests, debugging test failures, organizing test suites.

### terraform-stacks

**Content**: HCP Terraform Stacks configurations

**Key Files**:
- SKILL.md: High-level concepts, CLI commands, common patterns
- references/component-blocks.md: Complete `.tfcomponent.hcl` syntax
- references/deployment-blocks.md: Complete `.tfdeploy.hcl` syntax
- references/examples.md: Working examples from simple to complex

**Critical Concepts**:
- Stack Language is distinct from traditional Terraform HCL
- Components wrap modules (modules cannot contain providers)
- Deployments are isolated instances of the entire Stack
- Always use deployment groups, even for single deployments

**When to reference**: Multi-region/multi-environment infrastructure, HCP Terraform Stacks work.

### terraform-mcp-as-code

**Content**: MCP server skill for programmatic HCP Terraform operations

**Key Files**:
- SKILL.md: MCP server documentation, tool reference, security guidelines
- scripts/: TypeScript wrapper functions organized by category (34 tools total)

**Categories**:
- Organization: List orgs and projects
- Workspaces: Create, update, configure workspaces
- Variables: Manage workspace variables and variable sets
- Runs: Trigger and monitor Terraform runs
- Public Registry: Search modules, providers, policies
- Private Registry: Access private modules and providers

**When to reference**: Automating Terraform Cloud/Enterprise operations programmatically.

## Version Control and Git Workflow

This repository uses standard Git version control:

```bash
git status                    # Check current changes
git add <files>              # Stage changes
git commit -m "message"      # Commit changes
git push                     # Push to remote
```

**Important**: The main branch is `main` (not `master`).

Recent commits show the evolution:
- Added Terraform provider capabilities scripts
- Simplified installation instructions
- Enhanced documentation structure
- Fixed marketplace.json format to match Claude Code plugin structure

## Making Changes to Skills

### When Updating Documentation

1. **Maintain consistency across files**: If syntax changes, update SKILL.md, CLAUDE.md, and examples
2. **Validate all code examples**: Ensure HCL syntax is correct for the respective tool (Terraform vs Terraform Test vs Terraform Stacks)
3. **Update version requirements**: Note when features require specific Terraform versions
4. **Test installation flow**: Verify marketplace installation instructions remain accurate
5. **Keep terminology consistent**: Use exact terms (e.g., "run block" not "test block")

### When Adding New Skills

1. Create skill directory with required files: plugin.json, SKILL.md, CLAUDE.md, README.md
2. Add skill path to `.claude-plugin/marketplace.json` in the `skills` array
3. Follow existing naming conventions and file organization
4. Include version requirements and activation patterns
5. Test marketplace installation with new skill

### Documentation Quality Standards

**Required for all code examples**:
- Valid syntax for the specific technology
- Clear, descriptive variable/resource names
- Inline comments explaining key concepts
- Complete examples (not fragments unless explicitly noted)

**Required for all guidance**:
- Consistent terminology throughout
- Version-specific feature callouts
- Security best practices where applicable
- Common error patterns and solutions

## Common User Questions and Scenarios

### "How do I install these skills?"

Point to README.md installation section:
1. Add marketplace: `/plugin marketplace add hashi-demo-lab/claude-skill-hcp-terraform`
2. Install plugin: `/plugin install hcp-terraform-skills`
3. Skills activate automatically based on file type

### "Which skill should I use for X?"

- Writing Terraform configurations → terraform-style-guide
- Testing Terraform modules → terraform-test
- Multi-region/environment deployments → terraform-stacks
- Automating Terraform Cloud operations → terraform-mcp-as-code

### "Do these skills execute code?"

No. Skills are documentation/knowledge bases consumed by Claude Code. The terraform-mcp-as-code skill includes TypeScript wrapper functions, but these are reference implementations—users would need to set up the MCP server separately to execute them.

### "Can I contribute?"

Yes. The repository welcomes contributions:
- Documentation improvements
- New examples and patterns
- Bug fixes and clarifications

**Contribution requirements**:
1. Test all code examples
2. Maintain consistent terminology
3. Follow existing documentation structure

## Key Differences from Traditional Software Projects

**No Build System**:
- No compilation, transpilation, or bundling
- No package.json with dependencies
- No build/dist directories

**No Test Suites**:
- Code examples are documentation, not tested
- Quality assurance is manual verification
- No automated CI/CD for testing

**No Runtime Environment**:
- Skills are consumed by Claude Code, not executed
- No Docker containers (except for terraform-mcp-as-code MCP server)
- No deployment process

**Version Control Is for Documentation**:
- Git tracks documentation changes
- Releases are documentation snapshots
- No semantic versioning for breaking API changes

## Terraform Version Requirements

Skills reference different Terraform versions for specific features:

**terraform-test**:
- Terraform 1.6.0+: Terraform test introduced
- Terraform 1.7.0+: Mock providers added
- Terraform 1.9.0+: `state_key` and `parallel` attributes added

**terraform-stacks**:
- Requires HCP Terraform or Terraform Enterprise
- Not available in Terraform OSS CLI

**terraform-style-guide**:
- Terraform 1.0+: General applicability
- Specific formatting rules apply to all versions

## External Resources Referenced

- [Terraform Documentation](https://developer.hashicorp.com/terraform/docs)
- [Terraform Testing](https://developer.hashicorp.com/terraform/language/tests)
- [HCP Terraform Stacks](https://developer.hashicorp.com/terraform/cloud-docs/stacks)
- [Azure Verified Modules](https://azure.github.io/Azure-Verified-Modules/)

## Repository Metadata

- **License**: MIT
- **Owner**: Simon Lynch
- **Repository**: https://github.com/hashi-demo-lab/claude-skill-hcp-terraform
- **Marketplace Name**: hcp-terraform-skills
- **Version**: 1.0.0 (across all skills)

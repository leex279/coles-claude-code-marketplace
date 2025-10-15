# Context Engineering Marketplace & Plugin

This directory contains a complete Claude Code marketplace implementation with a powerful Context Engineering plugin that enhances your AI development workflow with specialized agents, custom commands, and hooks.

## Table of Contents

- [What's Included](#whats-included)
- [Quick Start](#quick-start)
- [Installation Methods](#installation-methods)
- [Plugin Features](#plugin-features)
- [Usage Guide](#usage-guide)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)

## What's Included

### Marketplace Structure
```
coles-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace definition
└── context-engineering-plugin/   # Main plugin directory
    ├── .claude-plugin/
    │   └── plugin.json           # Plugin manifest
    ├── agents/                   # Specialized AI agents
    │   ├── documentation-manager.md
    │   └── validation-gates.md
    ├── commands/                 # Custom slash commands
    │   ├── generate-prp.md
    │   ├── execute-prp.md
    │   ├── fix-github-issue.md
    │   ├── prep-parallel.md
    │   ├── execute-parallel.md
    │   └── primer.md
    ├── hooks/                    # Lifecycle hooks
    │   ├── README.md
    │   └── example-hook-config.json
    └── templates/
        └── prp_base.md          # PRP template
```

### Plugin Components

**1. Specialized Agents**
- `documentation-manager`: Manages project documentation, ensures consistency
- `validation-gates`: Validates code quality, runs tests, enforces standards

**2. Custom Commands**
- `/generate-prp`: Create comprehensive Product Requirements Prompts
- `/execute-prp`: Implement features from PRPs
- `/fix-github-issue`: Automatically fix GitHub issues
- `/prep-parallel`: Prepare parallel PRP variations
- `/execute-parallel`: Execute multiple PRPs in parallel
- `/primer`: Initialize projects with context engineering setup

**3. Hooks**
- PostToolUse hooks for automatic formatting
- PreToolUse hooks for validation
- Integration examples with validation agents

## Quick Start

### Installation Steps

1. **Add the marketplace from GitHub:**

```
/plugin marketplace add leex279/coles-claude-code-marketplace
```

2. **Install the plugin from the marketplace:**

```
/plugin install context-engineering-plugin@coles-claude-code-marketplace
```

3. **Restart Claude Code to load the plugin**

Exit and restart your Claude Code session.

4. **Verify the installation:**

```
/plugin marketplace list
```

You should see `coles-claude-code-marketplace` in the list.

```
/plugin list
```

You should see `context-engineering-plugin` in the list of installed plugins.

### Verify Installation

Check installed plugins:

```
/plugin list
```

You should see:
```
Installed plugins:
  - context-engineering-plugin v1.0.0
```

## Installation Command Reference

| Step | Command | Purpose |
|------|---------|---------|
| 1. Add Marketplace | `/plugin marketplace add leex279/coles-claude-code-marketplace` | Register the marketplace with Claude Code |
| 2. Install Plugin | `/plugin install context-engineering-plugin@coles-claude-code-marketplace` | Install the plugin from the marketplace |
| 3. Verify Installation | `/plugin list` | Check that the plugin is installed |

## Plugin Features

### 1. PRP Workflow Commands

The PRP (Product Requirements Prompt) workflow is the core of context engineering:

**Generate a PRP:**
```bash
# In Claude Code
/generate-prp INITIAL.md
```

This command:
- Reads your feature request from INITIAL.md
- Researches the codebase for patterns
- Searches for relevant documentation online
- Creates a comprehensive implementation blueprint
- Saves the PRP to `PRPs/{feature-name}.md`

**Execute a PRP:**
```bash
# In Claude Code
/execute-prp PRPs/your-feature-name.md
```

This command:
- Loads all context from the PRP
- Creates a detailed implementation plan
- Executes each step with validation
- Runs tests and fixes failures
- Ensures all success criteria are met

### 2. GitHub Integration

**Fix GitHub Issues Automatically:**
```bash
/fix-github-issue 123
```

This command:
- Fetches issue details using `gh` CLI
- Analyzes the issue and related code
- Creates a fix with tests
- Validates the fix
- Optionally creates a PR

### 3. Parallel Execution

**Prepare Parallel PRPs:**
```bash
/prep-parallel feature-name 3
```

Creates multiple PRP variations for testing different approaches.

**Execute PRPs in Parallel:**
```bash
/execute-parallel PRPs/feature-*.md
```

Runs multiple implementations concurrently for comparison.

### 4. Specialized Agents

The plugin includes two specialized agents that can be invoked via the Task tool:

**Documentation Manager:**
```python
# Automatically invoked when documentation tasks are needed
# Ensures consistency, updates README, maintains docs
```

**Validation Gates:**
```python
# Runs validation checks after code changes
# Includes linting, type checking, tests
# Enforces quality standards
```

### 5. Hooks System

The plugin includes example hooks that demonstrate:

- **Automatic formatting** after file edits
- **Validation checks** before tool execution
- **Integration with validation agents**

See `context-engineering-plugin/hooks/README.md` for detailed hook documentation.

## Usage Guide

### Setting Up a New Project

1. **Initialize with Primer:**
```bash
# In your project directory
/primer
```

This creates:
- `.claude/` directory structure
- `CLAUDE.md` with project rules
- `INITIAL.md` template
- `PRPs/` directory
- `examples/` directory

2. **Configure Project Rules:**

Edit `CLAUDE.md` to define:
- Code structure preferences
- Testing requirements
- Style conventions
- Documentation standards

3. **Add Examples:**

Place relevant code examples in `examples/`:
```
examples/
├── README.md           # Explains each example
├── api_client.py       # API integration pattern
├── database.py         # Database pattern
└── tests/
    └── test_example.py # Testing pattern
```

### Implementing a Feature

1. **Create Feature Request:**

Edit `INITIAL.md`:
```markdown
## FEATURE:
Build an async web scraper that extracts product data from e-commerce sites

## EXAMPLES:
- examples/api_client.py - Shows async/await patterns
- examples/database.py - Shows data persistence

## DOCUMENTATION:
- BeautifulSoup4: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
- aiohttp: https://docs.aiohttp.org/

## OTHER CONSIDERATIONS:
- Must handle rate limiting (max 10 req/sec)
- Store data in PostgreSQL
- Include retry logic for failed requests
```

2. **Generate PRP:**
```bash
/generate-prp INITIAL.md
```

3. **Review the PRP:**

Check `PRPs/web-scraper.md` for:
- Comprehensive implementation plan
- Validation gates
- Success criteria
- Context and documentation

4. **Execute the PRP:**
```bash
/execute-prp PRPs/web-scraper.md
```

5. **Verify Implementation:**

The AI will:
- Create all necessary files
- Write unit tests
- Run validation gates
- Fix any failures
- Confirm completion

## Configuration

### Marketplace Configuration

The marketplace is defined in `.claude-plugin/marketplace.json`:

```json
{
  "name": "coles-marketplace",
  "owner": {
    "name": "Dynamous"
  },
  "plugins": [
    {
      "name": "context-engineering-plugin",
      "source": "./context-engineering-plugin",
      "description": "Context Engineering plugin for Claude Code"
    }
  ]
}
```

### Plugin Configuration

The plugin manifest is in `context-engineering-plugin/.claude-plugin/plugin.json`:

```json
{
  "name": "context-engineering-plugin",
  "description": "Context Engineering Intro plugin featuring PRP workflow",
  "version": "1.0.0",
  "author": {
    "name": "Dynamous"
  }
}
```

### Hooks Configuration

To enable hooks, create `.claude/settings.json` in your project:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/format-after-edit.sh"
          }
        ]
      }
    ]
  }
}
```

See `context-engineering-plugin/hooks/example-hook-config.json` for more examples.

## Command Reference

### /generate-prp [file]
Generates a comprehensive PRP from a feature request file.

**Arguments:**
- `file`: Path to INITIAL.md or feature description file

**Output:**
- Creates `PRPs/{feature-name}.md`
- Includes all context, validation gates, and implementation plan

**Example:**
```bash
/generate-prp INITIAL.md
```

### /execute-prp [prp-file]
Executes a PRP to implement the feature.

**Arguments:**
- `prp-file`: Path to the PRP file to execute

**Process:**
1. Loads PRP context
2. Creates implementation plan
3. Executes all steps
4. Runs validation gates
5. Reports completion

**Example:**
```bash
/execute-prp PRPs/web-scraper.md
```

### /fix-github-issue [issue-number]
Automatically fixes a GitHub issue.

**Arguments:**
- `issue-number`: GitHub issue number

**Requirements:**
- `gh` CLI must be installed and authenticated

**Example:**
```bash
/fix-github-issue 123
```

### /prep-parallel [feature-name] [count]
Creates multiple PRP variations for parallel testing.

**Arguments:**
- `feature-name`: Name of the feature
- `count`: Number of variations to create (2-5 recommended)

**Example:**
```bash
/prep-parallel authentication 3
```

### /execute-parallel [pattern]
Executes multiple PRPs in parallel.

**Arguments:**
- `pattern`: Glob pattern matching PRP files

**Example:**
```bash
/execute-parallel PRPs/authentication-*.md
```

### /primer
Initializes a project with context engineering structure.

**Creates:**
- `.claude/` directory
- `CLAUDE.md` project rules
- `INITIAL.md` template
- `PRPs/` directory
- `examples/` directory

**Example:**
```bash
/primer
```

## Troubleshooting

### Plugin Not Found

**Issue:** `/plugin list` doesn't show the plugin

**Solutions:**
1. Verify installation inside Claude Code:
   ```
   /plugin list
   ```

2. Reinstall the plugin:
   ```
   /plugin remove context-engineering-plugin
   /plugin install context-engineering-plugin@coles-claude-code-marketplace
   ```

3. Check marketplace connection:
   ```
   /plugin marketplace list
   ```

### Commands Not Available

**Issue:** Slash commands like `/generate-prp` not recognized

**Solutions:**
1. Verify plugin is installed:
   ```
   /plugin list
   ```

2. Restart Claude Code session (type `/exit` then start new session)

3. Check if plugin loaded correctly:
   ```
   /help
   ```
   Look for the custom commands in the output

### Marketplace Installation Fails

**Issue:** `/plugin marketplace add leex279/coles-claude-code-marketplace` fails

**Solutions:**
1. Ensure GitHub repository is accessible
2. Check your network connection
3. Verify the repository URL is correct: `leex279/coles-claude-code-marketplace`
4. Try removing and re-adding:
   ```
   /plugin marketplace remove coles-claude-code-marketplace
   /plugin marketplace add leex279/coles-claude-code-marketplace
   ```

### Hooks Not Running

**Issue:** Hooks configured but not executing

**Solutions:**
1. Verify hook script is executable:
   ```bash
   chmod +x .claude/hooks/*.sh
   ```

2. Check settings.json syntax:
   ```bash
   cat .claude/settings.json
   ```

3. Enable debug mode:
   ```bash
   claude --debug
   ```

### PRP Generation Incomplete

**Issue:** Generated PRP missing context or validation gates

**Solutions:**
1. Ensure INITIAL.md is comprehensive
2. Add more examples to `examples/` folder
3. Include specific documentation URLs
4. Check that research phase completed successfully

## Advanced Usage

### Creating Custom Commands

1. Create a new markdown file in `commands/`:
```bash
touch context-engineering-plugin/commands/my-command.md
```

2. Define the command behavior:
```markdown
# My Custom Command

## Arguments: $ARGUMENTS

This command does something useful.

1. Read the arguments
2. Process the request
3. Generate output
```

3. The command is immediately available as `/my-command`

### Creating Custom Agents

1. Create an agent definition in `agents/`:
```bash
touch context-engineering-plugin/agents/my-agent.md
```

2. Define agent behavior and tools:
```markdown
# My Custom Agent

## Purpose
Specialized agent for specific tasks

## Tools
- Read
- Write
- Bash

## Process
1. Analyze input
2. Execute specialized logic
3. Validate results
```

3. Invoke via Task tool with subagent name

### Sharing Your Marketplace

1. **Publish to GitHub:**
```bash
git init
git add .
git commit -m "Add marketplace and plugin"
git remote add origin https://github.com/your-org/your-marketplace.git
git push -u origin main
```

2. **Share Installation Commands:**

Users can install your marketplace and plugins in Claude Code:

```bash
# Step 1: Add the marketplace
/plugin marketplace add your-org/your-marketplace

# Step 2: Install plugins from the marketplace
/plugin install your-plugin-name@your-marketplace
```

**Note:** Direct plugin installation without adding the marketplace first is not supported. Users must add the marketplace before installing plugins.

3. **Create Marketplace README:**
- Document all plugins
- Include usage examples with `/plugin` commands
- Provide troubleshooting guide
- Show both marketplace and direct install methods

## Best Practices

### For Plugin Development
1. **Keep commands focused** - One command, one purpose
2. **Include validation** - Always verify results
3. **Provide examples** - Show usage patterns
4. **Document thoroughly** - Explain what and why

### For Marketplace Management
1. **Version your plugins** - Use semantic versioning
2. **Test before sharing** - Verify all features work
3. **Maintain compatibility** - Keep plugins up to date
4. **Document changes** - Maintain a changelog

### For Context Engineering
1. **Be explicit** - Don't assume AI knows your preferences
2. **Provide examples** - Show patterns to follow
3. **Validate everything** - Include executable validation gates
4. **Iterate on PRPs** - Improve based on results

## Contributing

To contribute to this marketplace or plugin:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

See the LICENSE file in the root directory for licensing information.

## Resources

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Context Engineering Guide](../README.md)
- [Plugin Development Guide](https://docs.claude.com/en/docs/claude-code/plugins)
- [Marketplace Documentation](https://docs.claude.com/en/docs/claude-code/marketplace)

## Support

For issues or questions:
- Open an issue on GitHub
- Check the troubleshooting section
- Review Claude Code documentation
- Join the community discussions

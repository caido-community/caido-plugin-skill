# Caido Plugin Development Skill for Claude Code

**Comprehensive Caido plugin development guidance as a Claude Skill**

A [Claude Skill](https://www.anthropic.com/news/skills) that enables Claude to develop Caido plugins with expert knowledge of the SDK ecosystem, architecture patterns, and best practices. Packaged as a [Claude Code Plugin](https://docs.claude.com/en/docs/claude-code/plugins) for easy installation and distribution.

Claude autonomously uses this skill when you request Caido plugin development, loading comprehensive guidance for frontend, backend, and workflow plugins.

## Features

- **Complete SDK Coverage** - Frontend, Backend, and Workflow SDK documentation with code examples
- **Architecture Guidance** - Learn Caido's structure: Proxy, Intercept, Replay, Automate, and more
- **Plugin Types** - Frontend-only, backend-only, or full-stack plugin patterns
- **Quick Start Integration** - Uses official `pnpm create @caido-community/plugin` starter
- **Live References** - Links to latest SDK docs, popular plugins, and community resources
- **HTTPQL Knowledge** - Query language examples for filtering traffic
- **Best Practices** - TypeScript patterns, error handling, security, and performance tips
- **Example Plugins** - References to quickssrf, scanner, shift, authmatrix, and more

## Installation

### Option 1: Manual Git Clone

Install directly from GitHub to your skills directory:

**Global Installation (Available Everywhere):**
```bash
# Clone the repository
git clone https://github.com/caido-community/caido-plugin-skill.git /tmp/caido-plugin-skill

# Copy just the skill to your Claude skills directory
cp -r /tmp/caido-plugin-skill/skills/caido-plugin-dev ~/.claude/skills/

# Clean up
rm -rf /tmp/caido-plugin-skill
```

**Project-Specific Installation:**
```bash
# Clone the repository
git clone https://github.com/caido-community/caido-plugin-skill.git /tmp/caido-plugin-skill

# Copy just the skill to your project
mkdir -p /path/to/your/project/.claude/skills
cp -r /tmp/caido-plugin-skill/skills/caido-plugin-dev /path/to/your/project/.claude/skills/

# Clean up
rm -rf /tmp/caido-plugin-skill
```

### Option 2: Download Release

1. Download the latest release from [GitHub Releases](https://github.com/caido-community/caido-plugin-skill/releases)
2. Extract to:
   - Global: `~/.claude/skills/caido-plugin-dev`
   - Project: `/path/to/your/project/.claude/skills/caido-plugin-dev`
3. The skill is ready to use immediately (no build step required)

### Verify Installation

Run `/help` to confirm the skill is loaded, then ask Claude to "create a Caido plugin that does X" or "help me develop a Caido frontend plugin".

## Quick Start

After installation, simply ask Claude to help with Caido plugin development:

```
"Create a Caido plugin that detects SQL injection"
"Help me build a frontend plugin with a custom page"
"Show me how to intercept and modify requests in Caido"
"Create a plugin that integrates with an external API"
```

## Usage Examples

### Plugin Creation
```
"Create a new Caido plugin for SSRF detection"
"Build a plugin that adds a note-taking feature"
"Help me scaffold a full-stack Caido plugin"
```

### Frontend Development
```
"Add a custom page to Caido with a card layout"
"Create a command that appears in the command palette"
"Show me how to add context menu items to requests"
```

### Backend Development
```
"Write a backend plugin that queries saved requests"
"Show me how to send HTTP requests from a plugin"
"Create a plugin that generates security findings"
```

### Integration
```
"Build a plugin that integrates with Burp Collaborator"
"Create a workflow plugin for automation"
"Show me how to spawn external security tools"
```

## How It Works

1. Ask Claude about Caido plugin development
2. Claude loads the comprehensive skill with SDK documentation
3. Claude provides code examples, architecture guidance, and best practices
4. References link to latest official docs for up-to-date API information
5. Popular plugin examples show real-world implementation patterns

## What's Included

The skill provides:

- **Caido Platform Overview** - Understanding Proxy, Intercept, Replay, Automate, Workflows, etc.
- **Quick Start Guide** - Using `pnpm create @caido-community/plugin`
- **Configuration Files** - caido.config.ts and manifest.json examples
- **Frontend SDK** - UI components, pages, commands, context menus, storage, and more
- **Backend SDK** - HTTP operations, findings, intercept callbacks, storage, GraphQL, etc.
- **Runtime Modules** - Node.js-compatible modules available in QuickJS environment
- **Workflow SDK** - JavaScript nodes for automation sequences
- **Development Workflow** - Hot reload with Devtools, building, testing, debugging
- **Distribution** - Publishing to Caido Community Store
- **Popular Examples** - quickssrf, scanner, shift, authmatrix, workflows, and more
- **Best Practices** - TypeScript patterns, error handling, security, performance
- **Troubleshooting** - Common issues and solutions

## Learn More

- [Caido Developer Docs](https://developer.caido.io/)
- [Claude Code Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills)

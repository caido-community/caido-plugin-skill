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

This skill can be installed via the Claude Code plugin system or manually.

### Option 1: Via Plugin System (Recommended)

```bash
# Add this repository as a marketplace
/plugin marketplace add rez0/caido-plugin-skill

# Install the plugin
/plugin install caido-plugin-skill@caido-plugin-dev

# Verify installation
/help
```

Verify installation by running `/help` to confirm the skill is available.

### Option 2: Manual Git Clone

Install directly from GitHub to your skills directory:

**Global Installation (Available Everywhere):**
```bash
# Navigate to your Claude skills directory
cd ~/.claude/skills

# Clone the skill
git clone https://github.com/rez0/caido-plugin-skill.git

# The skill is ready to use (no build step required)
```

**Project-Specific Installation:**
```bash
# Install in a specific project
cd /path/to/your/project
mkdir -p .claude/skills
cd .claude/skills
git clone https://github.com/rez0/caido-plugin-skill.git
```

### Option 3: Download Release

1. Download the latest release from [GitHub Releases](https://github.com/rez0/caido-plugin-skill/releases)
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

## Dependencies

None - this is a pure documentation skill with no runtime dependencies.

For actual Caido plugin development, you'll need:
- Node.js 18+ or 20+
- pnpm package manager
- Caido application

## What is Caido?

Caido is a lightweight web security auditing toolkit designed for penetration testing and vulnerability assessment. It's similar to Burp Suite but built with modern technologies (Vue.js frontend, Rust backend) and an extensible plugin system.

Key features:
- HTTP/HTTPS proxy with traffic interception
- Request replay (repeater) and automation (fuzzer)
- Workflow system for multi-step automation
- Security findings management
- AI-powered assistant for attack suggestions
- Extensive plugin SDK for customization

## What is a Claude Skill?

[Skills](https://www.anthropic.com/news/skills) are modular capabilities that extend Claude's functionality. Unlike slash commands that you invoke manually, skills are model-invoked—Claude autonomously decides when to use them based on your request.

When you ask Claude to help with Caido plugin development, Claude discovers this skill, loads the comprehensive documentation, and provides expert guidance with code examples and references to official resources.

## Project Structure

```
caido-plugin-skill/
├── .github/
│   └── workflows/
│       └── release.yml       # Auto-creates releases with downloadable zip
├── skills/
│   └── caido-plugin-dev/     # The actual skill (Claude discovers this)
│       └── SKILL.md          # Comprehensive Caido plugin development guide
├── README.md                 # This file - user documentation
└── LICENSE                   # MIT License
```

## Skill Content

The SKILL.md file contains:
- 1000+ lines of comprehensive Caido plugin development documentation
- Complete SDK API coverage with code examples
- Architecture patterns and best practices
- Links to official documentation (always up-to-date)
- References to popular community plugins
- Troubleshooting and common issues

## SDK References (Always Current)

The skill links to official documentation to ensure you always have the latest API information:

- [Frontend SDK](https://developer.caido.io/reference/sdks/frontend/)
- [Backend SDK](https://developer.caido.io/reference/sdks/backend/)
- [Workflow SDK](https://developer.caido.io/reference/sdks/workflow/)
- [Runtime Modules](https://developer.caido.io/reference/modules/)
- [Developer Portal](https://developer.caido.io/)
- [LLM-Optimized Docs](https://developer.caido.io/llms.txt)

## Popular Plugin Examples

The skill references these community plugins as learning resources:

- **plugin-demo** - Official demo with Vue.js and TypeScript
- **Notebook** - Tutorial example with data persistence
- **quickssrf** - SSRF detection with external API integration
- **scanner** - Vulnerability scanning capabilities
- **shift** - AI integration into Caido
- **authmatrix** - Multi-user authorization testing
- **workflows** - Community automation workflows

## Learn More

- [Claude Skills](https://www.anthropic.com/news/skills) - Official announcement from Anthropic
- [Claude Code Skills Documentation](https://docs.claude.com/en/docs/claude-code/skills)
- [Claude Code Plugins Documentation](https://docs.claude.com/en/docs/claude-code/plugins)
- [Caido Developer Docs](https://developer.caido.io/)
- [Caido Community GitHub](https://github.com/caido-community)
- [GitHub Issues](https://github.com/rez0/caido-plugin-skill/issues)

## Contributing

Contributions are welcome! If you find issues with the documentation or want to add examples:

1. Fork the repository
2. Create a feature branch
3. Make your changes to `skills/caido-plugin-dev/SKILL.md`
4. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Caido](https://caido.io/) - For creating an amazing security testing platform
- [Caido Community](https://github.com/caido-community) - For excellent plugin examples
- Claude Code team - For the skills system

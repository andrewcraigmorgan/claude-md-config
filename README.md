# AI Coding Standards

![Version](https://img.shields.io/badge/version-1.0.0-blue)

Coding standards and guidelines for AI coding assistants (Claude Code, Cursor, Copilot, etc).

## Quick Setup

```bash
# Clone this repo
git clone https://github.com/andrewcraigmorgan/ai-coding-standards.git ~/ai-coding-standards

# For Claude Code: symlink the standards file
ln -sf ~/ai-coding-standards/AGENTS.md ~/.claude/CLAUDE.md

# Install custom skills
mkdir -p ~/.claude/skills
cp -r ~/ai-coding-standards/skills/* ~/.claude/skills/
```

## MCP Servers

The standards reference these MCP servers. Add to `~/.mcp.json`:

| MCP Server | Package | Purpose |
| ---------- | ------- | ------- |
| Bitbucket | `@aashari/mcp-server-atlassian-bitbucket` | PR management, repo operations |
| Zoho Projects | [zoho-projects-mcp](https://github.com/andrewcraigmorgan/zoho-projects-mcp) | Task and project management |

## Custom Skills

Reusable workflow instructions located in `skills/` directory. These work with any AI assistant.

| Skill | Purpose |
| ----- | ------- |
| `bitbucket-helpers` | PR creation with default reviewers, merge with branch cleanup |
| `zoho-task` | Task workflow automation based on Zoho task status |

For Claude Code, copy to `~/.claude/skills/`. For other assistants, reference the markdown files directly.

## What's Included

### Core Standards
- **Attribution rules** - Prevents AI from adding itself as a contributor
- **Bitbucket workflow** - PR creation and merging standards
- **Git standards** - Branch naming and commit message conventions
- **Coding standards** - TypeScript/JavaScript linting rules and style guide
- **Security standards** - Best practices for secure code
- **GDPR compliance** - Data protection, consent, user rights
- **Performance standards** - Guidelines for efficient code

### Framework Standards
- **Laravel conventions** - Controllers, Models, Migrations, Policies
- **React/Next.js conventions** - Components, hooks, server components
- **Vue conventions** - Composition API, composables

### Frontend Standards
- **CSS/Tailwind** - Utility-first patterns and breakpoints
- **SCSS/BEM** - Block-Element-Modifier methodology
- **Accessibility (WCAG 2.1 AA)** - Semantic HTML, ARIA, keyboard navigation
- **SEO** - Meta tags, Open Graph, structured data, Core Web Vitals
- **Images & Assets** - WebP, lazy loading, srcset, optimization

### Quality & Process
- **Environment management** - Local → Staging → Production workflow
- **Code review checklist** - Self-review and reviewer guidelines
- **Testing standards** - Unit, integration, E2E requirements
- **Browser support matrix** - Supported browsers and breakpoints

### Backend Standards
- **Database conventions** - Migration naming, column conventions, soft deletes

## For Other AI Assistants

Reference `AGENTS.md` in your project root or include it in your assistant's context.

## Customization

Edit `AGENTS.md` to match your team's standards and workflows.

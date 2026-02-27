# Global Instructions

**Standards Version: 2.0.0**

- Never add AI/agent as a contributor in any files (package.json, README, etc.)
- Never include copyright notices or attribution claiming AI wrote the code
- All code written belongs to the user

## External Communications

**CRITICAL:** Always ask for user approval before posting to external systems:

- **Zoho comments** - Never post task comments without showing the user first and getting explicit approval
- **PR comments** - Show the user the comment before posting
- **PR descriptions** - Show the user the PR body before creating
- **Slack/email** - Never send messages without approval
- **Any client-visible content** - Always get approval first

**Formatting rules for all external communications:**

- **No emojis** - Never use emojis, checkmarks, or decorative characters
- **Professional tone** - Plain text, no filler or fluff
- **Proper markdown** - Blank lines after headings, one list item per line

When you need to post a comment or update an external system:
1. Draft the content
2. Show it to the user
3. Ask "Would you like me to post this?"
4. Only proceed after explicit approval

## Token Efficiency

Minimize token usage to reduce costs:

### Response Style
- **Be concise** - No preamble ("I'll help you with that..."), just do it
- **Don't repeat back** - Don't echo the user's request, just act
- **Skip obvious explanations** - Don't explain what `git commit` does
- **No filler** - Avoid "Great question!", "Absolutely!", "Let me..."

### File Operations
- **Read targeted sections** - Use `offset` and `limit` for large files
- **Grep before reading** - Find the line numbers first, then read only that section
- **Don't re-read files** - Remember what you've already read in this session

### Search Strategy
- **Glob before Grep** - Find files by name pattern first if you know it
- **Use specific patterns** - `class UserService` not just `User`
- **Limit results** - Use `head_limit` on searches

### Code Output
- **Edit don't rewrite** - Use Edit tool for changes, not Write for whole files
- **Minimal diffs** - Only show changed code, not surrounding context
- **Skip unchanged files** - Don't output files you didn't modify

### Planning
- **Act on simple tasks** - Don't plan for single-file changes
- **Batch questions** - Ask multiple clarifications at once, not one at a time
- **Trust context** - Don't re-explore code you've already seen

## Version Check

When asked "what version of coding standards are you using?" or similar:

1. Report the version shown above (Standards Version: X.X.X)
2. Check if updates are available: `git -C ~/ai-coding-standards fetch && git -C ~/ai-coding-standards log HEAD..origin/main --oneline`
3. If updates exist, offer to pull: `git -C ~/ai-coding-standards pull`

## Required MCP Servers

| MCP Server | Package/Repository | Purpose |
| ---------- | ------------------ | ------- |
| Bitbucket | `@aashari/mcp-server-atlassian-bitbucket` | PR management, repo operations |
| Zoho Projects | [zoho-projects-mcp](https://github.com/andrewcraigmorgan/zoho-projects-mcp) | Task and project management |

### Installation

Add to `~/.mcp.json`:

```json
{
  "mcpServers": {
    "bitbucket": {
      "command": "npx",
      "args": ["-y", "@aashari/mcp-server-atlassian-bitbucket"],
      "env": {
        "ATLASSIAN_USER_EMAIL": "your-email@example.com",
        "ATLASSIAN_API_TOKEN": "your-api-token"
      }
    },
    "zoho-projects": {
      "command": "node",
      "args": ["/path/to/zoho-projects-mcp/dist/index.js"],
      "env": {
        "ZOHO_CLIENT_ID": "your-client-id",
        "ZOHO_CLIENT_SECRET": "your-client-secret",
        "ZOHO_REFRESH_TOKEN": "your-refresh-token",
        "ZOHO_PORTAL_ID": "your-portal-id",
        "ZOHO_API_DOMAIN": "https://projectsapi.zoho.com"
      }
    }
  }
}
```

## Browser Automation (Playwright)

Use **Playwright CLI** instead of MCP - it's ~4x more token-efficient (27k vs 114k tokens per task).

```bash
npm install -g @playwright/cli
npx playwright install
```

Reference: [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)

## Custom Skills

Copy to `~/.claude/skills/`:

| Skill | Purpose |
| ----- | ------- |
| `bitbucket-helpers` | PR creation with default reviewers, merge with branch cleanup |
| `zoho-task` | Task workflow automation based on Zoho task status |

## Bitbucket

Use the Bitbucket MCP tools (`mcp__bitbucket__bb_*`) for all Bitbucket operations.

### Creating PRs
1. Fetch default reviewers: `bb_get /repositories/{ws}/{repo}/default-reviewers`
2. Filter out the author (Andrew Morgan / andrewmorganmtc) - authors can't review their own PR
3. Create PR with remaining reviewers, destination branch `staging`

### Merging PRs
- **Wait for CI pipeline to pass before merging**
- Always use `"close_source_branch": true`
- Use `"merge_strategy": "merge_commit"`

### PR Comments Formatting

- **No emojis** - Never use checkmarks, crosses, or any emoji characters
- **Blank lines required** - Add blank line after headings and between list items
- **One item per line** - Each numbered/bulleted item must be on its own line

## Git Workflow

### Branch Naming

```
feature/add-checklist-scheduling
bugfix/fix-authentication-redirect
hotfix/critical-security-patch
chore/update-dependencies
refactor/restructure-components
```

### Commit Message Format

```
type(scope): description

Types: feat, fix, docs, style, refactor, test, chore

Examples:
feat(api): add checklist instance generation endpoint
fix(mobile): resolve authentication token refresh issue
```

## TypeScript/JavaScript Linting

**CRITICAL:** Follow these rules to avoid pre-commit failures:

1. **No `console.log`** - Only `console.warn` and `console.error` allowed
2. **No unused variables** - Use `catch {` instead of `catch (error) {` if error unused
3. **No unused imports** - Remove any imports that aren't used
4. **Run lint before committing** - `npm run lint`

### Code Style

| Rule | Value |
| ---- | ----- |
| Indentation | 4 spaces |
| Max line length | 120 characters |
| JS/TS semicolons | None |
| JS/TS quotes | Single quotes |
| JS/TS trailing commas | Always |

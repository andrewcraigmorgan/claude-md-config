# Global Instructions

**Standards Version: 1.0.1**

- Never add AI/agent as a contributor in any files (package.json, README, etc.)
- Never include copyright notices or attribution claiming AI wrote the code
- All code written belongs to the user

## External Communications

**CRITICAL:** Always ask for user approval before posting to external systems:

- **Zoho comments** - Never post task comments without showing the user first and getting explicit approval
- **PR descriptions** - Show the user the PR body before creating
- **Slack/email** - Never send messages without approval
- **Any client-visible content** - Always get approval first

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

## AI Setup Assistant

**When a user clones this repo or references these standards, proactively check if required services are configured:**

1. **Check for MCP servers**: If Bitbucket or Zoho tools are needed but not available, guide the user through setup
2. **Check for skills**: If skills are referenced but not installed, offer to copy them to `~/.claude/skills/`
3. **Check for symlinks**: If using Claude Code, offer to symlink AGENTS.md to `~/.claude/CLAUDE.md`

### Quick Setup Commands

```bash
# Clone this repo
git clone https://github.com/andrewcraigmorgan/ai-coding-standards.git ~/ai-coding-standards

# Symlink for Claude Code
ln -sf ~/ai-coding-standards/AGENTS.md ~/.claude/CLAUDE.md

# Install skills
cp -r ~/ai-coding-standards/skills/* ~/.claude/skills/
```

## Required MCP Servers

Install these MCP servers for full functionality:

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

## Custom Skills

Reusable workflow instructions for AI assistants. Located in the `skills/` directory of this repo.

| Skill | Purpose |
| ----- | ------- |
| `bitbucket-helpers` | PR creation with default reviewers, merge with branch cleanup |
| `zoho-task` | Task workflow automation based on Zoho task status |

### Installation

**Claude Code:** Copy to `~/.claude/skills/`
```bash
cp -r skills/* ~/.claude/skills/
```

**Other AI assistants:** Reference the skill markdown files directly or include in context.

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

## Git Workflow Standards

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

- feat: new features
- fix: bug fixes
- docs: documentation changes
- style: formatting changes
- refactor: code restructuring
- test: adding/updating tests
- chore: maintenance tasks

Examples:
feat(api): add checklist instance generation endpoint
fix(mobile): resolve authentication token refresh issue
```

## Coding Standards

### TypeScript/JavaScript Linting Rules

**CRITICAL:** Follow these rules to avoid pre-commit failures:

1. **No `console.log`** - Only `console.warn` and `console.error` are allowed. For tests, remove console statements entirely.

2. **No unused variables** - Every declared variable must be used. In catch blocks, use `catch {` (no variable) instead of `catch (error) {` if you don't use the error.

3. **No unused imports** - Remove any imports that aren't used in the file.

4. **Run lint before committing** - Always run `npm run lint` to check for issues before staging files.

```typescript
// BAD - will fail pre-commit
console.log('Debug info')
try { ... } catch (error) { /* error unused */ }

// GOOD
console.warn('Warning message')  // or remove entirely
try { ... } catch { /* no variable needed */ }
```

### Quick Reference

| Rule                  | Value              |
| --------------------- | ------------------ |
| Indentation           | 4 spaces (no tabs) |
| Max line length       | 120 characters     |
| Line endings          | LF (Unix-style)    |
| PHP standard          | PSR-12             |
| JS/TS semicolons      | None               |
| JS/TS quotes          | Single quotes      |
| JS/TS trailing commas | Always             |

## Security Standards

- **Never store plaintext passwords** - always use proper hashing
- **Environment variables** for secrets (never commit .env files)
- **Input validation** on all requests
- **Authorization checks** in all controllers
- **Use UUIDs** instead of predictable IDs where appropriate

## GDPR Compliance

### Core Principles

All applications handling EU user data must comply with GDPR. Build privacy into every feature.

- **Lawfulness** - Have a valid legal basis for processing (consent, contract, legal obligation, legitimate interest)
- **Purpose limitation** - Only collect data for specified, explicit purposes
- **Data minimization** - Collect only what is strictly necessary
- **Accuracy** - Keep data accurate and up to date
- **Storage limitation** - Don't keep data longer than necessary
- **Integrity & confidentiality** - Protect data with appropriate security

### Privacy by Design

Build privacy into systems from the start, not as an afterthought:

- **Default to privacy** - Most restrictive settings by default
- **Minimal data collection** - Question every field: "Do we really need this?"
- **Purpose-specific** - Don't repurpose data without new consent
- **Encryption** - Encrypt PII at rest and in transit
- **Access controls** - Limit who can access personal data

### Consent Management

```typescript
// Consent must be:
// - Freely given (no pre-ticked boxes)
// - Specific (separate consent for each purpose)
// - Informed (clear explanation of what they're agreeing to)
// - Unambiguous (clear affirmative action)
// - Withdrawable (as easy to withdraw as to give)

interface UserConsent {
    marketing_emails: boolean
    analytics_tracking: boolean
    third_party_sharing: boolean
    consented_at: Date
    ip_address: string
    consent_version: string  // Track which privacy policy version
}
```

### Data Subject Rights

Implement endpoints/functionality for all GDPR rights:

| Right | Implementation |
| ----- | -------------- |
| Access (Art. 15) | Export all user data in machine-readable format |
| Rectification (Art. 16) | Allow users to correct their data |
| Erasure (Art. 17) | Hard delete or anonymize all user data |
| Restrict Processing (Art. 18) | Flag to stop processing while disputes resolved |
| Data Portability (Art. 20) | Export data in JSON/CSV format |
| Object (Art. 21) | Opt-out of profiling and direct marketing |

### Data Retention Periods

Define and enforce retention periods for all data types:

| Data Type | Retention | Action After |
| --------- | --------- | ------------ |
| Active user accounts | While active + 2 years | Anonymize or delete |
| Inactive accounts | 2 years of inactivity | Email warning, then delete |
| Transaction records | 7 years (legal requirement) | Archive, restrict access |
| Server logs | 90 days | Auto-delete |
| Analytics data | 26 months | Aggregate and anonymize |
| Marketing consent | Until withdrawn | Delete on withdrawal |
| Support tickets | 3 years after resolution | Anonymize |

### Cookie Compliance

```typescript
// Cookie categories - user must consent to non-essential
enum CookieCategory {
    ESSENTIAL,      // No consent needed (auth, security, preferences)
    ANALYTICS,      // Requires consent
    MARKETING,      // Requires consent
    THIRD_PARTY     // Requires consent
}

// Requirements:
// - Show cookie banner before setting non-essential cookies
// - Allow granular consent (not just "accept all")
// - Provide easy way to change preferences
// - Don't use cookie walls (blocking content until consent)
// - Document all cookies in privacy policy
```

### Data Breach Procedures

In case of a data breach:

1. **Detect & contain** - Stop the breach, preserve evidence
2. **Assess severity** - What data? How many users? What risk?
3. **Notify ICO** - Within 72 hours if risk to individuals (UK)
4. **Notify users** - Without undue delay if high risk
5. **Document** - Record all breaches, even minor ones
6. **Review** - Update security measures to prevent recurrence

### International Data Transfers

Data leaving the UK/EU requires additional safeguards:

- **Adequacy decision** - Transfer freely to approved countries
- **Standard Contractual Clauses (SCCs)** - Required for US transfers
- **Document transfers** - Know where all data flows
- **Third-party due diligence** - Verify vendors are GDPR compliant

### Third-Party Data Processing

When using external services that process personal data:

- **Data Processing Agreement (DPA)** - Must be signed before sharing data
- **Vendor assessment** - Verify security measures and GDPR compliance
- **Document sub-processors** - Know the full chain of data handling
- **Include in privacy policy** - Disclose all third parties to users

### Privacy Policy Requirements

Every application must have a privacy policy that includes:

- [ ] Identity and contact details of data controller
- [ ] Types of data collected
- [ ] Purposes and legal basis for processing
- [ ] Data retention periods
- [ ] Third parties data is shared with
- [ ] International transfers
- [ ] User rights and how to exercise them
- [ ] Right to complain to supervisory authority
- [ ] Cookie policy
- [ ] Last updated date

## Performance Standards

- **Eager loading** to prevent N+1 queries
- **Database indexes** on foreign keys and frequently queried columns
- **Pagination** for large result sets
- **Queue jobs** for heavy operations

## Framework Standards

### Laravel Conventions

- **Controllers** - Single responsibility, use Form Requests for validation
- **Models** - Keep business logic in Services or Actions, not Models
- **Migrations** - Descriptive names: `create_users_table`, `add_status_to_orders_table`
- **Seeders** - Use factories for test data, seeders for essential data only
- **Routes** - Use resource routes where possible, group by middleware
- **Policies** - Always use policies for authorization, never inline checks

```php
// BAD
if ($user->id === $post->user_id) { ... }

// GOOD
$this->authorize('update', $post);
```

### React/Next.js Conventions

- **Components** - Functional components with hooks, no class components
- **File naming** - PascalCase for components (`UserCard.tsx`), kebab-case for utilities (`format-date.ts`)
- **Props** - Define TypeScript interfaces, destructure in function signature
- **State** - Local state for UI, global state (Context/Zustand) for shared data
- **Server Components** - Default to server components in Next.js App Router, use `'use client'` only when needed

```tsx
// Component structure
interface UserCardProps {
    user: User
    onSelect?: (id: string) => void
}

export function UserCard({ user, onSelect }: UserCardProps) {
    // hooks first
    // handlers second
    // render last
}
```

### Vue Conventions

- **Composition API** - Use `<script setup>` syntax
- **Props** - Define with TypeScript using `defineProps<T>()`
- **Emits** - Always type emits with `defineEmits<T>()`
- **Composables** - Extract reusable logic to `use*` functions

## CSS & Styling Standards

### Tailwind CSS

- **Component extraction** - Extract repeated patterns to components, not `@apply`
- **Arbitrary values** - Avoid `w-[137px]`, use design system values
- **Dark mode** - Use `dark:` variants consistently
- **Responsive** - Mobile-first, use `sm:`, `md:`, `lg:`, `xl:` breakpoints

```html
<!-- Breakpoints -->
sm: 640px
md: 768px
lg: 1024px
xl: 1280px
2xl: 1536px
```

### SCSS/BEM Methodology

When not using Tailwind:

```scss
// Block
.card { }

// Element
.card__header { }
.card__body { }
.card__footer { }

// Modifier
.card--featured { }
.card--compact { }
```

## Accessibility Standards (WCAG 2.1 AA)

### Required Practices

- **Semantic HTML** - Use correct elements (`<button>`, `<nav>`, `<main>`, `<article>`)
- **Alt text** - All images must have descriptive alt text (or `alt=""` for decorative)
- **Focus states** - Visible focus indicators on all interactive elements
- **Keyboard navigation** - All functionality accessible via keyboard
- **Color contrast** - Minimum 4.5:1 for normal text, 3:1 for large text
- **Form labels** - Every input must have an associated label
- **Error messages** - Clear, descriptive error messages linked to inputs
- **Skip links** - Provide "Skip to main content" link

```html
<!-- BAD -->
<div onclick="submit()">Submit</div>

<!-- GOOD -->
<button type="submit">Submit</button>
```

### ARIA Usage

- Use ARIA only when semantic HTML is insufficient
- Never use `role="button"` on a `<div>` - use `<button>`
- Always include `aria-label` for icon-only buttons

## SEO Standards

### Required Meta Tags

```html
<title>Page Title - Site Name</title>
<meta name="description" content="155-160 character description">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="canonical" href="https://example.com/page">

<!-- Open Graph -->
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Description">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="website">

<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image">
```

### Structured Data

- Add JSON-LD schema for: Organization, LocalBusiness, Product, Article, BreadcrumbList
- Validate with Google Rich Results Test

### Core Web Vitals Targets

| Metric | Target |
| ------ | ------ |
| LCP (Largest Contentful Paint) | < 2.5s |
| INP (Interaction to Next Paint) | < 200ms |
| CLS (Cumulative Layout Shift) | < 0.1 |

## Image & Asset Standards

### Image Optimization

- **Format** - WebP with JPEG/PNG fallback for older browsers
- **Lazy loading** - Use `loading="lazy"` for below-fold images
- **Responsive images** - Use `srcset` and `sizes` attributes
- **Aspect ratio** - Always set `width` and `height` to prevent CLS
- **Max file sizes** - Hero: 200KB, thumbnails: 50KB, icons: 5KB

```html
<img
    src="image.webp"
    srcset="image-400.webp 400w, image-800.webp 800w, image-1200.webp 1200w"
    sizes="(max-width: 768px) 100vw, 50vw"
    width="800"
    height="600"
    loading="lazy"
    alt="Descriptive alt text"
>
```

### Asset Organization

```
public/
├── images/
│   ├── hero/
│   ├── icons/
│   └── content/
├── fonts/
└── favicons/
```

## Environment & Deployment

### Environment Promotion

```
local → staging → production
```

- **Never push directly to main/master**
- **All changes go through staging first**
- **Production deployments require PR approval**

### Environment Variables

```bash
# Required in all environments
APP_ENV=local|staging|production
APP_DEBUG=true|false  # false in production
APP_URL=https://example.com

# Staging-specific
STAGING_BASIC_AUTH=true
ROBOTS_NOINDEX=true
```

### Pre-deployment Checklist

- [ ] All tests passing
- [ ] Migrations reviewed
- [ ] Environment variables set
- [ ] Cache cleared/warmed
- [ ] Assets compiled
- [ ] Backup verified

## Third-Party Integrations

### Payment Integration

- **Never log card data** - PCI compliance
- **Idempotency keys** - Prevent duplicate charges
- **Webhook verification** - Always verify webhook signatures

### Analytics

- **Cookie consent** - Load analytics only after consent
- **Event tracking** - Document all custom events
- **Privacy** - Anonymize IP addresses where required by law

## Code Review Checklist

### Before Requesting Review

- [ ] Self-reviewed the diff
- [ ] Tests added/updated
- [ ] Linting passes
- [ ] No console.log statements
- [ ] No commented-out code
- [ ] No hardcoded values that should be config

### Reviewer Checklist

- [ ] Logic is correct and handles edge cases
- [ ] No security vulnerabilities (SQL injection, XSS, etc.)
- [ ] Performance implications considered
- [ ] Accessibility maintained
- [ ] Mobile responsive
- [ ] Error handling appropriate
- [ ] Code is readable and maintainable

## Testing Standards

### Test Types Required

| Type | Coverage | Tools |
| ---- | -------- | ----- |
| Unit | Business logic, utilities | PHPUnit, Jest, Vitest |
| Integration | API endpoints, database | PHPUnit, Supertest |
| E2E | Critical user flows | Cypress, Playwright |

### What to Test

- **Unit** - Pure functions, calculations, transformations
- **Integration** - API responses, database queries, auth flows
- **E2E** - Checkout, sign up, login, critical forms

### Naming Convention

```typescript
describe('UserService', () => {
    it('should return user by id when user exists', () => { })
    it('should throw NotFoundError when user does not exist', () => { })
})
```

## Browser & Device Support

### Supported Browsers

| Browser | Version |
| ------- | ------- |
| Chrome | Last 2 versions |
| Firefox | Last 2 versions |
| Safari | Last 2 versions |
| Edge | Last 2 versions |
| iOS Safari | Last 2 versions |
| Samsung Internet | Last 2 versions |

**Not supported:** Internet Explorer (any version)

### Responsive Breakpoints

| Name | Min Width | Target Devices |
| ---- | --------- | -------------- |
| Mobile | 0 | Phones |
| Tablet | 768px | Tablets, small laptops |
| Desktop | 1024px | Laptops, desktops |
| Large | 1280px | Large monitors |

## Database Standards

### Migration Naming

```
YYYY_MM_DD_HHMMSS_action_table_description.php

Examples:
2024_01_15_100000_create_users_table.php
2024_01_15_100001_add_status_to_orders_table.php
2024_01_15_100002_drop_legacy_columns_from_products_table.php
```

### Column Conventions

- **Primary key** - `id` (auto-increment or UUID)
- **Foreign keys** - `{table}_id` (e.g., `user_id`)
- **Timestamps** - `created_at`, `updated_at`
- **Soft deletes** - `deleted_at`
- **Booleans** - `is_*` or `has_*` prefix (e.g., `is_active`, `has_verified_email`)
- **Dates** - `*_at` suffix for timestamps, `*_date` for dates only

### Soft Delete Policy

- **Users** - Soft delete (GDPR data retention)
- **Content** - Soft delete (recoverable)
- **Logs/Analytics** - Hard delete after retention period
- **Transactional data** - Never delete, use status flags

### Right to Be Forgotten (GDPR Article 17)

All systems must implement a hard delete capability for user data upon request:

- **User data purge** - Implement a method to permanently erase all personal data
- **Cascade deletion** - Remove user data from all related tables and backups
- **Anonymization alternative** - Where deletion breaks referential integrity, anonymize instead (replace PII with hashed/random values)
- **Audit trail** - Log that a deletion request was fulfilled (without logging deleted data)
- **Third-party notification** - Notify integrated services to delete user data
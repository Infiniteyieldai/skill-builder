# Composio Integration Patterns for Claude Skills

This reference covers how to build Claude skills that wrap Composio-powered SaaS automations. Use these patterns when your skill needs to take real actions in external apps (send emails, create issues, post Slack messages, update spreadsheets, etc.).

---

## What is Composio?

Composio provides authenticated integrations to 500+ SaaS apps via a unified MCP (Model Context Protocol) server. Skills that use Composio can act on behalf of the user in real apps without storing credentials themselves.

**Core concept:** Your skill orchestrates *what to do*; Composio handles *how to authenticate and execute it*.

---

## Prerequisites

Before a Composio-powered skill can run, the user needs:

1. A free Composio API key from [platform.composio.dev](https://platform.composio.dev)
2. The `connect-apps-plugin` installed:
   ```bash
   claude --plugin-dir ~/.claude/skills/connect-apps-plugin
   ```
3. Setup completed:
   ```
   /connect-apps:setup
   ```
4. Per-app OAuth authorization (happens automatically on first use of each app)

> Always document these prerequisites in your skill's `SKILL.md`.

---

## Skill Structure for Composio Skills

```
my-composio-skill/
├── SKILL.md                          # Core skill (< 500 lines)
├── supported-apps.md                 # List of apps this skill works with
├── authentication-setup.md           # Setup guide for first-time users
└── scripts/
    └── validate-connection.js        # Node.js script to check auth status
```

---

## SKILL.md Pattern for Composio Skills

```yaml
---
name: sending-slack-notifications
description: Use this skill when sending messages, notifications, or updates to Slack channels or direct messages. This includes posting announcements, alert notifications, status updates, or automated reports to Slack. Invoke when users want to send a Slack message, notify a team, post to a channel, or create a Slack DM. Requires Composio setup (see authentication-setup.md).
---

# Sending Slack Notifications

Sends messages to Slack channels and direct messages using the Composio Slack integration.

## Prerequisites

Before using this skill, ensure Composio is configured. See `./authentication-setup.md` for setup instructions.

## How to Use

1. Identify the target: channel name (e.g., `#general`) or user (`@username`)
2. Draft the message content
3. Use the Composio Slack tool to send

## Example Actions

**Post to a channel:**
```
Send "Deploy successful for v2.1.0" to #deployments
```

**Send a DM:**
```
DM @john.smith with the meeting summary
```

**Rich message with blocks:**
```
Post to #alerts: Title "Server CPU High", Body "CPU at 94% on prod-1", Color red
```

## Supported Operations

- Post message to public/private channel
- Send direct message to user
- Reply to a thread
- Post with attachments or blocks
- Schedule a message (if supported by Composio plan)

## Error Handling

- **Not authorized**: Run `/connect-apps:setup` and authorize Slack
- **Channel not found**: Verify channel name spelling, check membership
- **Rate limited**: Wait 1 minute and retry

## Reference

For the full list of supported Slack operations, see `./supported-apps.md`.
```

---

## Authentication Setup Template

Use this as the base for `authentication-setup.md` in any Composio skill:

```markdown
# Authentication Setup

This skill uses Composio to connect to [App Name]. Follow these steps once per machine:

## Step 1: Get Your Composio API Key

1. Go to [platform.composio.dev](https://platform.composio.dev)
2. Sign up or log in (free account)
3. Copy your API key from the dashboard

## Step 2: Configure the Plugin

```bash
claude --plugin-dir ~/.claude/skills/connect-apps-plugin
```

When prompted, run:
```
/connect-apps:setup
```

Paste your Composio API key when asked.

## Step 3: Authorize [App Name]

The first time Claude attempts an action in [App Name], you'll be prompted to authorize via OAuth. Follow the browser prompt to grant access.

## Verify Setup

Ask Claude:
```
Check if my [App Name] connection is working
```

Claude will confirm the connection or prompt you to re-authorize.
```

---

## Node.js Validation Script

Add `scripts/validate-connection.js` to help users verify their Composio setup:

```javascript
#!/usr/bin/env node
/**
 * validate-connection.js
 * Checks if the Composio connection for a given app is active.
 * Usage: node validate-connection.js [app-name]
 */
import { execSync } from 'child_process';

const app = process.argv[2] || 'slack';

try {
  // Query Composio API for active connections
  const result = execSync(
    `curl -s -H "x-api-key: ${process.env.COMPOSIO_API_KEY}" \
    https://backend.composio.dev/api/v1/connectedAccounts?appName=${app}`,
    { encoding: 'utf8' }
  );

  const data = JSON.parse(result);
  const connections = data.items || [];
  const active = connections.filter(c => c.status === 'ACTIVE');

  if (active.length > 0) {
    console.log(`✅ ${app} connection active (${active.length} account(s))`);
    active.forEach(c => console.log(`   - ${c.accountId}`));
  } else {
    console.log(`❌ No active ${app} connection found.`);
    console.log('   Run /connect-apps:setup to authorize.');
  }
} catch (err) {
  console.error('Error checking connection:', err.message);
  console.error('Ensure COMPOSIO_API_KEY is set in your environment.');
  process.exit(1);
}
```

---

## Common Composio App Patterns

### Email (Gmail / Outlook)

```yaml
name: sending-email-drafts
description: Use this skill when composing and sending emails through Gmail or Outlook. Invoke when users want to send an email, draft a message, reply to an email thread, or forward a message to someone. Requires Composio Gmail/Outlook authorization.
```

**Key actions:** `GMAIL_SEND_EMAIL`, `GMAIL_CREATE_DRAFT`, `OUTLOOK_SEND_EMAIL`

---

### GitHub

```yaml
name: managing-github-issues
description: Use this skill when creating, updating, or closing GitHub issues, assigning issues to team members, adding labels, or commenting on existing issues. Invoke when working with GitHub issue tracking, bug reports, feature requests, or project management via GitHub. Requires Composio GitHub authorization.
```

**Key actions:** `GITHUB_CREATE_AN_ISSUE`, `GITHUB_UPDATE_AN_ISSUE`, `GITHUB_ADD_COMMENT_TO_ISSUE`

---

### Notion

```yaml
name: updating-notion-pages
description: Use this skill when creating, reading, or updating Notion pages and databases. Invoke when users want to add content to Notion, create a new page, update a database entry, or search Notion content. Requires Composio Notion authorization.
```

**Key actions:** `NOTION_CREATE_PAGE`, `NOTION_UPDATE_PAGE`, `NOTION_QUERY_DATABASE`

---

### Google Sheets

```yaml
name: writing-to-spreadsheets
description: Use this skill when reading from or writing to Google Sheets spreadsheets. This includes appending rows, updating cells, reading data ranges, or creating new sheets. Invoke when users want to log data, track metrics, update a spreadsheet, or export data to Google Sheets. Requires Composio Google Sheets authorization.
```

**Key actions:** `GOOGLESHEETS_BATCH_UPDATE`, `GOOGLESHEETS_GET_SPREADSHEET_INFO`, `GOOGLESHEETS_SHEET_FROM_JSON`

---

## Naming Convention for Composio Skills

Since Composio skills perform actions (not just analysis), use action-focused gerunds:

| Good | Bad |
|------|-----|
| `sending-slack-notifications` | `slack-skill` |
| `creating-github-issues` | `github-helper` |
| `updating-notion-databases` | `notion-connector` |
| `managing-calendar-events` | `calendar-tool` |
| `syncing-crm-contacts` | `crm-integration` |

---

## Description Checklist for Composio Skills

In addition to standard skill metadata requirements, Composio skills should include:

- [ ] Mention the specific app(s) by name
- [ ] List at least 3 concrete action types
- [ ] Include "Requires Composio [App] authorization" at the end
- [ ] Trigger keywords include both the app name and action verbs

---

## Multi-App Orchestration Pattern

When a skill needs to coordinate across multiple apps (e.g., create a GitHub issue AND notify Slack):

```yaml
name: orchestrating-saas-workflows
description: Use this skill when automating multi-step workflows that span multiple SaaS apps. This includes creating cross-platform notifications (GitHub issue → Slack message), syncing data between services (Airtable → Google Sheets), or triggering sequences of actions across tools. Invoke when users describe a workflow like "when X happens in app A, do Y in app B". Requires Composio setup with relevant apps authorized.
```

**Pattern:** Decompose into sequential single-app actions, confirm each step with the user before executing.

---

## Error Recovery Patterns

| Error | Recovery Strategy |
|-------|------------------|
| `Not authorized` | Prompt user to run `/connect-apps:setup` |
| `App not connected` | Guide user to authorize via OAuth |
| `Rate limit` | Exponential backoff (2s, 4s, 8s, 16s) |
| `Invalid field` | Show required fields for the action |
| `Quota exceeded` | Upgrade Composio plan or wait for reset |

Always surface errors clearly — never silently swallow Composio API failures.

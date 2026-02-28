# MCP Integration Patterns for Claude Skills

This reference covers how to build Claude skills that invoke MCP (Model Context Protocol) servers. Use these patterns when your skill needs capabilities beyond file I/O — database access, browser automation, web search, external APIs — that are exposed through MCP tools.

---

## What is MCP?

MCP is a protocol that lets Claude Code connect to external capability servers. Each MCP server exposes a set of **tools** that Claude can call, similar to function calls. Skills can leverage MCP tools to extend what Claude can do without hardcoding API calls.

**Key distinction from Composio:**
- **MCP** = direct server-to-tool protocol (lower level, more flexible)
- **Composio** = managed SaaS integrations with auth handling (higher level, easier)

Use MCP when you need precise control, custom tools, or capabilities not in Composio's catalogue.

---

## Available MCP Categories (from claude-code-templates)

| Category | Example Servers | Use For |
|----------|----------------|---------|
| `browser_automation` | Playwright MCP, Puppeteer MCP | Web scraping, testing, screenshots |
| `database` | Supabase MCP, PostgreSQL MCP, SQLite MCP | Direct DB queries |
| `deepresearch` | Perplexity MCP, Tavily MCP | Web search, research |
| `devtools` | GitHub MCP, GitLab MCP | Code/repo operations |
| `filesystem` | Filesystem MCP | File system access |
| `integration` | Slack MCP, Linear MCP | Direct app integration |
| `productivity` | Google Calendar MCP, Notion MCP | Productivity tools |
| `web` | Fetch MCP, Web search MCP | HTTP requests, scraping |
| `audio` | ElevenLabs MCP | TTS, audio generation |
| `marketing` | HubSpot MCP, Mailchimp MCP | Marketing automation |

---

## Skill Structure for MCP Skills

```
my-mcp-skill/
├── SKILL.md                          # Core skill (< 500 lines)
├── mcp-server-setup.md               # How to install the required MCP server
├── available-tools.md                # List of MCP tools this skill uses
└── scripts/
    └── check-mcp-status.js           # Verify MCP server is running
```

---

## SKILL.md Pattern for MCP Skills

```yaml
---
name: querying-supabase-databases
description: Use this skill when querying, inserting, updating, or managing data in a Supabase database. This includes running SQL queries, managing rows in tables, working with Supabase Auth users, or inspecting database schema. Invoke when users mention Supabase, PostgreSQL queries in a Supabase project, or working with database records. Requires the Supabase MCP server to be configured.
---

# Querying Supabase Databases

Uses the Supabase MCP server to execute queries and manage data in your Supabase project.

## Prerequisites

Ensure the Supabase MCP server is installed and configured. See `./mcp-server-setup.md`.

## Capabilities

This skill uses the following MCP tools:
- `execute_sql` — Run raw SQL queries
- `list_tables` — Show all tables in the schema
- `get_table_schema` — Describe a table's columns
- `insert_rows` — Insert new records
- `update_rows` — Update existing records

See `./available-tools.md` for the complete tool list.

## Workflow

1. Understand what the user needs (query, insert, update, schema info)
2. Identify the target table(s)
3. Confirm sensitive operations (DELETE, UPDATE without WHERE) with the user
4. Execute using the appropriate MCP tool
5. Present results clearly

## Example Queries

**List all tables:**
```sql
SELECT table_name FROM information_schema.tables WHERE table_schema = 'public';
```

**Count recent signups:**
```sql
SELECT COUNT(*) FROM auth.users WHERE created_at > NOW() - INTERVAL '7 days';
```

**Insert a record:**
```sql
INSERT INTO products (name, price, stock) VALUES ('Widget Pro', 29.99, 100);
```

## Safety Rules

- Always show the SQL before executing destructive operations
- Require explicit confirmation for DELETE and DROP statements
- Never expose connection strings or service role keys in output
```

---

## MCP Server Setup Template

Use as the base for `mcp-server-setup.md`:

```markdown
# MCP Server Setup: [Server Name]

This skill requires the [Server Name] MCP server. Follow these steps to install it.

## Installation

Add to your Claude Code MCP configuration (`~/.claude/mcp.json` or `.claude/mcp.json`):

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"],
      "env": {
        "API_KEY": "your-api-key-here"
      }
    }
  }
}
```

## Required Environment Variables

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| `API_KEY` | Service API key | https://service.com/settings/api |

## Verify Installation

Restart Claude Code, then ask:
```
List available MCP tools
```

You should see tools from `server-name` in the list.

## Troubleshooting

- **Server not found**: Run `npx -y @package/mcp-server --version` to verify install
- **Auth error**: Double-check your API key in mcp.json
- **Port conflict**: Change the port in the server args
```

---

## Available Tools Documentation Template

Use as the base for `available-tools.md`:

```markdown
# Available MCP Tools: [Server Name]

Tools exposed by the [Server Name] MCP server that this skill uses.

## Tool Reference

### `tool_name`
**Description:** What this tool does
**When to use:** Specific scenario
**Parameters:**
- `param1` (string, required): Description
- `param2` (number, optional): Description, default: 10

**Example:**
```json
{
  "tool": "tool_name",
  "params": {
    "param1": "value",
    "param2": 5
  }
}
```

**Returns:** Description of the response structure
```

---

## MCP Tool Invocation Patterns

### Pattern 1: Read-Only Data Fetch

Best for: Database queries, API data retrieval, file reads

```markdown
## Workflow

1. Parse user's data request
2. Determine which MCP tool to use
3. Call tool with appropriate parameters
4. Format and present results
5. Offer follow-up actions (export, filter, visualize)
```

---

### Pattern 2: Write Operation with Confirmation

Best for: Database writes, file mutations, API posts

```markdown
## Workflow

1. Understand the intended change
2. Show the user what will be executed (SQL, API payload, file diff)
3. Ask for confirmation: "This will [action]. Proceed? (yes/no)"
4. Only execute after explicit confirmation
5. Report success/failure with affected record count
```

---

### Pattern 3: Browser Automation

Best for: Web scraping, form submission, screenshot capture, e2e testing

```markdown
## Workflow

1. Navigate to the target URL using the browser MCP
2. Wait for page load (check for specific element)
3. Execute interactions (click, fill, submit)
4. Extract data or capture screenshot
5. Close browser session when done

## Example using Playwright MCP

- Navigate: `browser_navigate` with URL
- Click: `browser_click` with selector
- Fill form: `browser_fill` with selector and value
- Screenshot: `browser_screenshot`
- Extract: `browser_evaluate` with JavaScript expression
```

---

### Pattern 4: Research and Synthesis

Best for: Web research, fact-checking, competitive analysis

```markdown
## Workflow

1. Decompose user's research question into sub-queries
2. Run parallel searches using web/research MCP tools
3. Deduplicate and filter results by relevance
4. Synthesize findings into a structured summary
5. Cite sources with URLs
```

---

## MCP vs. Direct API Calls

Choose the right approach:

| Situation | Use MCP | Use Direct API (curl/fetch) |
|-----------|---------|---------------------------|
| Auth is complex (OAuth, refresh tokens) | ✅ | ❌ |
| Standard CRUD on known service | ✅ | ✅ |
| Custom endpoint not in MCP | ❌ | ✅ |
| Need real-time streaming | ❌ | ✅ |
| Rapid prototyping / scripting | ❌ | ✅ |
| Production skill (reliable auth) | ✅ | ❌ |

---

## MCP Server Configuration Locations

| Scope | File Location | When to Use |
|-------|--------------|-------------|
| Global (all projects) | `~/.claude/mcp.json` | Personal tools you always want |
| Project-specific | `.claude/mcp.json` | Team-shared project tooling |
| Inline in skill | Document in `mcp-server-setup.md` | Skill documents its own requirements |

---

## Naming Convention for MCP Skills

Action-focused names that include the technology:

| Good | Bad |
|------|-----|
| `querying-supabase-databases` | `supabase-skill` |
| `scraping-web-pages` | `web-scraper` |
| `searching-with-perplexity` | `research-tool` |
| `managing-github-repos` | `github-helper` |
| `automating-browser-tasks` | `browser-bot` |

---

## Error Handling for MCP Skills

```markdown
## Error Handling

| Error | Cause | Recovery |
|-------|-------|---------|
| `MCP server not found` | Server not installed/configured | Point to mcp-server-setup.md |
| `Tool not available` | Wrong MCP version | Update with `npm update @package/mcp-server` |
| `Auth failed` | Invalid/expired credentials | Re-check API key in mcp.json |
| `Timeout` | Server unresponsive | Restart Claude Code to reset MCP connections |
| `Schema mismatch` | API version changed | Check server changelog, update skill docs |
```

---

## Security Considerations

1. **Never log secrets** — don't echo API keys, tokens, or passwords in skill output
2. **Confirm destructive operations** — always ask before DELETE, DROP, or PATCH
3. **Scope permissions** — document the minimum scopes required for the MCP server
4. **Validate inputs** — sanitize user-provided values before passing to MCP tools
5. **Audit trail** — for write operations, log what was changed and when

---

## Testing MCP Skills

Before publishing a skill that uses MCP:

```javascript
// scripts/check-mcp-status.js
#!/usr/bin/env node
/**
 * Verifies that required MCP server tools are available.
 */
import { execSync } from 'child_process';

const requiredTools = [
  'tool_one',
  'tool_two',
  'tool_three',
];

// Claude Code exposes MCP tools via claude mcp list
try {
  const output = execSync('claude mcp list --json', { encoding: 'utf8' });
  const tools = JSON.parse(output);
  const toolNames = tools.map(t => t.name);

  let allPresent = true;
  for (const tool of requiredTools) {
    if (toolNames.includes(tool)) {
      console.log(`✅ ${tool}`);
    } else {
      console.log(`❌ ${tool} — not found`);
      allPresent = false;
    }
  }

  if (!allPresent) {
    console.log('\nSome tools are missing. See ./mcp-server-setup.md');
    process.exit(1);
  }

  console.log('\nAll required MCP tools are available.');
} catch (err) {
  console.error('Could not list MCP tools:', err.message);
  process.exit(1);
}
```

# API Skill Template

Copy this template when building a skill that integrates with an external REST or GraphQL API. Replace all `[PLACEHOLDERS]` with actual values.

---

## File Structure to Create

```
[api-name]-integration/
├── SKILL.md                              # This template → your main skill file
├── [api-name]-endpoint-reference.md      # Key endpoints, auth, rate limits
├── [api-name]-authentication-setup.md    # How to get and configure credentials
└── scripts/
    └── test-[api-name]-connection.js     # Verify API key works
```

---

## SKILL.md Content

```yaml
---
name: [gerund-action]-with-[api-name]
description: Use this skill when [primary use case for this API]. This includes [specific operation 1], [specific operation 2], [specific operation 3], and [specific operation 4]. Invoke when users mention [api-name], [key term 1], [key term 2], or want to [common user phrasing]. Requires a [API Name] API key (see authentication-setup.md).
---

# [Action] with [API Name]

[One-sentence summary of what this skill does and why it's useful.]

## Prerequisites

You need a [API Name] API key. See `./[api-name]-authentication-setup.md` for setup.

## Supported Operations

| Operation | Description | When to Use |
|-----------|-------------|-------------|
| [Operation 1] | [What it does] | [Trigger scenario] |
| [Operation 2] | [What it does] | [Trigger scenario] |
| [Operation 3] | [What it does] | [Trigger scenario] |

## Workflow

1. Identify the operation the user needs
2. Gather required parameters (ask if missing)
3. Check credentials are configured
4. Execute the API call
5. Present results in a readable format
6. Offer follow-up actions

## Authentication

Set your API key:
```bash
export [API_NAME]_API_KEY="your-key-here"
```

Or add to `.env`:
```
[API_NAME]_API_KEY=your-key-here
```

## Example: [Most Common Operation]

```javascript
// scripts/[operation-name].js
import fetch from 'node:fetch';

const API_KEY = process.env.[API_NAME]_API_KEY;
const BASE_URL = '[https://api.example.com/v1]';

async function [operationName](params) {
  const response = await fetch(`${BASE_URL}/[endpoint]`, {
    method: 'GET', // or POST, PUT, DELETE
    headers: {
      'Authorization': `Bearer ${API_KEY}`,
      'Content-Type': 'application/json',
    },
    // body: JSON.stringify(params), // for POST/PUT
  });

  if (!response.ok) {
    const error = await response.json();
    throw new Error(`API error ${response.status}: ${error.message}`);
  }

  return response.json();
}

// Main execution
const result = await [operationName]({ /* params */ });
console.log(JSON.stringify(result, null, 2));
```

## Error Reference

| HTTP Status | Meaning | Resolution |
|-------------|---------|-----------|
| 401 | Unauthorized | Check API key is set and valid |
| 403 | Forbidden | Verify API key has required permissions |
| 404 | Not Found | Check resource ID or endpoint path |
| 429 | Rate Limited | Wait before retrying (check `Retry-After` header) |
| 500 | Server Error | Retry after 30s; check [api-name] status page |

## Rate Limits

[API Name] limits requests to [X] per [minute/hour/day] on the [Free/Pro] plan.

For full endpoint reference, rate limits, and advanced patterns, see `./[api-name]-endpoint-reference.md`.
```

---

## [api-name]-authentication-setup.md Content

```markdown
# [API Name] Authentication Setup

## Step 1: Create an Account

Go to [https://[api-name].com/signup] and create a free account.

## Step 2: Generate an API Key

1. Log in to the [API Name] dashboard
2. Navigate to **Settings → API Keys**
3. Click **Create New Key**
4. Name it (e.g., "Claude Code")
5. Copy the key immediately — it won't be shown again

## Step 3: Configure Your Environment

**Option A: Environment variable (recommended for dev)**
```bash
export [API_NAME]_API_KEY="your-key-here"
# Add to ~/.bashrc or ~/.zshrc to persist
```

**Option B: .env file (for project-specific use)**
```
[API_NAME]_API_KEY=your-key-here
```
> Add `.env` to your `.gitignore` — never commit API keys!

**Option C: Claude Code secrets (for team sharing)**
```bash
claude config set secrets.[API_NAME]_API_KEY your-key-here
```

## Step 4: Verify

Run the verification script:
```bash
node scripts/test-[api-name]-connection.js
```

Expected output:
```
✅ [API Name] connection successful
   Account: your@email.com
   Plan: Free
   Requests remaining: 950/1000
```

## Troubleshooting

- **Invalid key**: Make sure you copied the full key with no trailing spaces
- **Forbidden**: Your plan may not include this endpoint — check [api-name].com/pricing
- **Key not found**: Ensure the env var name matches exactly: `[API_NAME]_API_KEY`
```

---

## [api-name]-endpoint-reference.md Content

```markdown
# [API Name] Endpoint Reference

Base URL: `https://api.[api-name].com/v[N]`
Auth: `Authorization: Bearer {API_KEY}`

## Endpoints Used by This Skill

### GET /[resource]
**Description:** [What this returns]
**Parameters:**
- `limit` (int, optional): Max results, default 20
- `offset` (int, optional): Pagination offset
- `filter` (string, optional): Filter expression

**Response:**
```json
{
  "data": [...],
  "total": 100,
  "page": 1
}
```

### POST /[resource]
**Description:** [What this creates]
**Body:**
```json
{
  "field1": "value",
  "field2": 42
}
```

**Response:**
```json
{
  "id": "abc123",
  "created_at": "2026-01-01T00:00:00Z"
}
```

## Rate Limits

| Plan | Requests/min | Requests/day |
|------|-------------|-------------|
| Free | 10 | 100 |
| Pro | 100 | 10,000 |
| Enterprise | 1,000 | Unlimited |

## Webhook Events (if applicable)

| Event | Trigger |
|-------|---------|
| `resource.created` | New resource created |
| `resource.updated` | Existing resource changed |
| `resource.deleted` | Resource removed |

## Official Docs

[API Name] official documentation: [https://docs.[api-name].com]
```

---

## scripts/test-[api-name]-connection.js Content

```javascript
#!/usr/bin/env node
/**
 * test-[api-name]-connection.js
 * Verifies that the [API Name] API key is set and working.
 * Usage: node scripts/test-[api-name]-connection.js
 */
import 'dotenv/config';

const API_KEY = process.env.[API_NAME]_API_KEY;

if (!API_KEY) {
  console.error('❌ [API_NAME]_API_KEY is not set.');
  console.error('   See ./[api-name]-authentication-setup.md for instructions.');
  process.exit(1);
}

try {
  const response = await fetch('https://api.[api-name].com/v1/[me-or-status-endpoint]', {
    headers: {
      'Authorization': `Bearer ${API_KEY}`,
      'Content-Type': 'application/json',
    },
  });

  if (!response.ok) {
    const err = await response.json().catch(() => ({}));
    console.error(`❌ API returned ${response.status}: ${err.message || 'Unknown error'}`);
    process.exit(1);
  }

  const data = await response.json();
  console.log('✅ [API Name] connection successful');
  console.log(`   Account: ${data.email || data.id || 'authenticated'}`);
  console.log(`   Plan: ${data.plan || 'unknown'}`);
} catch (err) {
  console.error('❌ Connection failed:', err.message);
  process.exit(1);
}
```

---

## Checklist Before Publishing

- [ ] Skill name is in gerund form: `[gerund]-with-[api-name]`
- [ ] Description starts with "Use this skill when..."
- [ ] Description includes 5+ trigger keywords
- [ ] Description under 1024 characters
- [ ] Auth setup documented in `[api-name]-authentication-setup.md`
- [ ] Endpoints documented in `[api-name]-endpoint-reference.md`
- [ ] Test script in `scripts/test-[api-name]-connection.js`
- [ ] Error codes documented in SKILL.md
- [ ] Rate limits documented
- [ ] No API keys hardcoded anywhere
- [ ] `.env` mentioned in `.gitignore`
- [ ] No `allowed-tools`, `model`, or `tools` in YAML frontmatter

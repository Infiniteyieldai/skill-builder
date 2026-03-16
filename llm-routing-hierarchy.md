# LLM Routing Hierarchy & Token Cost Strategy

## Current Status (2026-03-16)

> **⚠️ Anthropic Credits: DEPLETED** — `cachedExtraUsageDisabledReason: "out_of_credits"`
> Top up at https://console.anthropic.com/settings/billing

> **⚠️ No third-party API keys found** — `.env.shared` file does not exist yet.
> See Section 5 to create it.

---

## 1. The Problem: Token Cost Hierarchy

You have access to 4 LLM providers. Not all work is equal — don't burn
Claude Opus/Sonnet tokens on tasks that a cheaper model handles fine.

```
COST (high → low)          CAPABILITY (high → low)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claude Opus 4.6            Complex reasoning, architecture, security, code review
Claude Sonnet 4.6          General coding, multi-step tasks, analysis
GPT-4o / Gemini 1.5 Pro    Drafting, summarisation, structured output
Gemini Flash / GPT-4o-mini Fast, cheap: formatting, classification, simple Q&A
Grok 2 / Grok 3            Real-time web data, Twitter/X context, news
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 2. Routing Decision Framework

### Use Claude Opus 4.6 for:
- Architecture decisions and system design
- Security audits and vulnerability analysis
- Complex multi-file refactors
- Debugging hard/novel problems
- Writing production-critical code
- Reasoning about ambiguous requirements
- Creating new skills, agents, or Claude config

### Use Claude Sonnet 4.6 for:
- Standard coding tasks (new features, bug fixes)
- Code explanation and documentation
- Multi-step CLI workflows
- PR reviews on non-critical code
- Generating tests
- **Default Claude Code model** (already your current model)

### Use GPT-4o / Gemini 1.5 Pro for:
- Drafting README files, blog posts, docs
- Summarising long documents or logs
- Structured JSON/YAML generation from examples
- Marketing copy, email drafts
- Translating between formats (CSV→JSON, etc.)
- First-pass code scaffolding to refine with Claude

### Use Gemini 2.0 Flash / GPT-4o-mini for:
- Classifying or tagging items in bulk
- Simple regex/pattern matching tasks
- Formatting and linting output
- Checking spelling/grammar
- Generating commit message drafts
- Answering FAQ-style questions
- Parsing and extracting structured data from text

### Use Grok 2/3 for:
- Real-time crypto news and sentiment (feeds your crypto-gem-scanner)
- Twitter/X trend analysis
- Current events context
- Social signal detection for trading
- Time-sensitive market research

---

## 3. n8n Routing Workflow

Your Automation-stack uses n8n. Build an LLM Router workflow:

```
[Trigger: Task arrives]
        ↓
[Classify task complexity] ← use Gemini Flash (cheap classifier)
        ↓
  ┌─────┴──────┐
  │ complexity? │
  └─────┬──────┘
        ├── "critical/complex"  → Claude Opus 4.6
        ├── "standard/code"     → Claude Sonnet 4.6
        ├── "draft/format"      → GPT-4o or Gemini Pro
        ├── "bulk/simple"       → Gemini Flash / GPT-4o-mini
        └── "realtime/social"   → Grok API
```

### n8n Node Setup
Each provider needs an HTTP Request node or use the built-in AI nodes:
- **Anthropic**: n8n has native Anthropic node
- **OpenAI**: n8n has native OpenAI node
- **Google Gemini**: n8n has native Google AI node
- **Grok/xAI**: HTTP Request node → `https://api.x.ai/v1/chat/completions`

---

## 4. Claude Code Model Selection

Claude Code supports per-task model selection via sub-agents and skills.

### In settings.json — set default model:
```json
{
  "model": "claude-sonnet-4-6",
  "smallFastModel": "claude-haiku-4-5-20251001"
}
```

### In sub-agents — specify model per agent:
```yaml
---
name: security-auditor
model: claude-opus-4-6
description: Deep security analysis requiring highest capability
---
```

```yaml
---
name: commit-message-writer
model: claude-haiku-4-5-20251001
description: Fast, cheap commit message generation
---
```

### Task routing in CLAUDE.md:
Tell Claude when to use which model by putting rules in your global CLAUDE.md:

```markdown
## Model Selection Rules
- Use Opus only for: security analysis, architecture, novel debugging
- Default to Sonnet for: all standard coding work
- Use Haiku for: commit messages, simple formatting, quick lookups
- Delegate to external LLMs via n8n for: drafting, docs, bulk tasks
```

---

## 5. Create the .env.shared File

This file should live in a location accessible across your projects.
Suggested path: `~/.claude/.env.shared`

```bash
# ~/.claude/.env.shared
# LLM Provider API Keys
# DO NOT COMMIT THIS FILE

# Anthropic (Claude)
ANTHROPIC_API_KEY=sk-ant-...          # from console.anthropic.com

# OpenAI (GPT-4o, GPT-4o-mini)
OPENAI_API_KEY=sk-proj-...            # from platform.openai.com

# Google (Gemini Pro, Gemini Flash)
GOOGLE_API_KEY=AIza...                # from aistudio.google.com
GEMINI_API_KEY=AIza...                # same key, different env var name

# xAI (Grok 2, Grok 3)
XAI_API_KEY=xai-...                   # from console.x.ai

# GitHub (for MCP server + gh CLI)
GITHUB_TOKEN=ghp_...                  # from github.com/settings/tokens

# n8n (if using API)
N8N_API_KEY=...
N8N_WEBHOOK_URL=https://your-n8n-instance/webhook/...

# Telegram (crypto-gem-scanner alerts)
TELEGRAM_BOT_TOKEN=...
TELEGRAM_CHAT_ID=...
```

### Wire into Claude Code settings.json:
```json
{
  "model": "claude-sonnet-4-6",
  "env": {
    "ANTHROPIC_API_KEY": "${ANTHROPIC_API_KEY}",
    "OPENAI_API_KEY": "${OPENAI_API_KEY}",
    "GOOGLE_API_KEY": "${GOOGLE_API_KEY}",
    "XAI_API_KEY": "${XAI_API_KEY}",
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

### Load in shell profile:
```bash
# Add to ~/.bashrc or ~/.zshrc
if [ -f ~/.claude/.env.shared ]; then
  set -a
  source ~/.claude/.env.shared
  set +a
fi
```

---

## 6. Model Cost Reference (as of 2026)

| Model | Input (per 1M tokens) | Output (per 1M tokens) | Best For |
|-------|----------------------|------------------------|----------|
| Claude Opus 4.6 | ~$15 | ~$75 | Critical work only |
| Claude Sonnet 4.6 | ~$3 | ~$15 | Default coding |
| Claude Haiku 4.5 | ~$0.25 | ~$1.25 | Simple tasks |
| GPT-4o | ~$2.50 | ~$10 | Docs, drafts |
| GPT-4o-mini | ~$0.15 | ~$0.60 | Bulk/cheap |
| Gemini 1.5 Pro | ~$1.25 | ~$5 | Long context |
| Gemini 2.0 Flash | ~$0.075 | ~$0.30 | Fastest/cheapest |
| Grok 2 | ~$2 | ~$10 | Real-time data |

**Rule of thumb:** Gemini Flash is ~200x cheaper than Opus for the same token count.
Use it as your default for anything that doesn't need deep reasoning.

---

## 7. Practical Workflow Examples

### Example: Crypto gem scanner enrichment
```
1. DEXScreener token detected (n8n trigger)
2. Grok → get Twitter/X sentiment for token  [cheap, real-time]
3. Gemini Flash → classify risk level          [cheap classifier]
4. IF high risk: Claude Sonnet → deeper analysis [mid cost]
5. Telegram alert with combined output
```

### Example: GitHub PR handling
```
1. PR opened (GitHub webhook → n8n)
2. Gemini Flash → summarise what changed       [cheap]
3. GPT-4o-mini → generate first-pass review    [cheap]
4. IF security-related: Claude Opus → deep audit [expensive, justified]
5. Post review comment via GitHub MCP
```

### Example: Content Machine (TikTok)
```
1. Video topic decided
2. GPT-4o → write script draft                 [cheap]
3. Gemini Flash → generate hashtags/captions   [very cheap]
4. Claude Sonnet → review + refine final copy  [only if needed]
5. Post via TikTok API
```

---

## 8. Action Plan

- [ ] Create `~/.claude/.env.shared` with your API keys
- [ ] Add `source ~/.claude/.env.shared` to shell profile
- [ ] Update `~/.claude/settings.json` with env section
- [ ] Create `~/.claude/CLAUDE.md` with model routing rules (see report)
- [ ] Top up Anthropic credits at console.anthropic.com
- [ ] Build n8n LLM router workflow in Automation-stack
- [ ] Create Haiku sub-agent for cheap/fast tasks in Claude Code
- [ ] Register for xAI API at console.x.ai (feed into crypto-gem-scanner)

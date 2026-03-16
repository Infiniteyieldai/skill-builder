# LLM Routing Hierarchy & Token Cost Strategy
# Projects: GoMining | Content Machine | Alpha Core | Tax Agent | Gmail Agent | Maintenance Super | Agentic Trader

## Current Status (2026-03-16)
> **⚠️ Anthropic Credits: DEPLETED** — top up at https://console.anthropic.com/settings/billing
> **⚠️ .env.shared missing** — create at `~/.claude/.env.shared` (see Section 5)

---

## 1. Cost Tiers

```
TIER 1 — EXPENSIVE (Claude)        Use sparingly, high-value work only
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Claude Opus 4.6    ~$15/$75 per 1M   Critical logic, security, architecture
Claude Sonnet 4.6  ~$3/$15 per 1M    Default coding model
Claude Haiku 4.5   ~$0.25/$1.25/1M   Fast tasks, boilerplate, commits

TIER 2 — MID (OpenAI / Google Pro)  Good for content, docs, drafts
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GPT-4o             ~$2.50/$10 /1M    Scripts, email drafts, content
Gemini 1.5 Pro     ~$1.25/$5  /1M    Long docs (ATO PDFs, tax law)

TIER 3 — CHEAP (Flash / Mini)       Default for bulk and simple tasks
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GPT-4o-mini        ~$0.15/$0.60/1M   Captions, hashtags, classification
Gemini 2.0 Flash   ~$0.075/$0.30/1M  Cheapest useful model — batch work

TIER 4 — REAL-TIME (Grok)          For live market/social data
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Grok 2/3           ~$2/$10  /1M     Twitter/X, crypto sentiment, news
```

---

## 2. Per-Project Routing

### GoMining Reel Generation
`/Users/aidengoode/Claude-Antigravity/GoMining Reel Generation/`

| Task | Model | Reason |
|------|-------|--------|
| Video script writing | GPT-4o | Creative content, mid-cost |
| Caption + hashtag generation | Gemini Flash | Bulk, repetitive — cheapest |
| Reel scheduling logic (code) | Claude Sonnet | Standard code task |
| Video pipeline architecture | Claude Sonnet | Standard code task |
| Voiceover text variations (bulk) | Gemini Flash | Many variants cheaply |

---

### Content Machine (TikTok Automation)
`/Users/aidengoode/Claude-Antigravity/content-machine/`

| Task | Model | Reason |
|------|-------|--------|
| TikTok post scripts | GPT-4o | Trending tone, good at short-form |
| Hashtag research/generation | Gemini Flash | Cheap batch task |
| TikTok API integration code | Claude Sonnet | Standard coding |
| Content strategy planning | GPT-4o | Creative planning |
| Bulk caption variants (A/B) | Gemini Flash | Many cheap variants |
| Post scheduling logic | Claude Sonnet | Code task |

---

### Alpha Core — Autonomous Income Ecosystem
`/Users/aidengoode/Claude-Antigravity/Alpha Core- Autonomous Income Ecosystem/`

| Task | Model | Reason |
|------|-------|--------|
| Agent orchestration design | Claude Opus | Architecture — most critical project |
| Inter-agent communication logic | Claude Sonnet | Standard complex code |
| Income stream monitoring code | Claude Sonnet | Standard code |
| Status reports / summaries | Gemini Flash | Cheap summary generation |
| Security audit of agent system | Claude Opus | Security-critical |
| New agent integration | Claude Sonnet | Standard code |
| High-level strategy decisions | Claude Opus | Requires deep reasoning |

---

### Australian Tax Agent
`/Users/aidengoode/Claude-Antigravity/Australian Tax Agent/`

| Task | Model | Reason |
|------|-------|--------|
| ATO tax law interpretation | Claude Opus | Legal accuracy critical |
| Tax calculation logic (code) | Claude Sonnet | Standard code |
| Summarising ATO PDF documents | Gemini 1.5 Pro | Long context (128k), cheap |
| Tax form filling logic | Claude Sonnet | Standard code |
| Compliance rule validation | Claude Opus | High-stakes, accuracy required |
| User-facing tax explanations | GPT-4o | Clear plain-English output |
| Batch transaction categorisation | Gemini Flash | High volume, cheap |

---

### Gmail Agent
`/Users/aidengoode/Claude-Antigravity/Gmail Agent/`

| Task | Model | Reason |
|------|-------|--------|
| Email triage/classification | Gemini Flash | Bulk classification, very cheap |
| Email reply drafting | GPT-4o | Natural tone, good at email |
| Gmail API integration code | Claude Sonnet | Standard code |
| Priority inbox rules (code) | Claude Sonnet | Standard code |
| Urgent/sensitive email handling | Claude Sonnet | Context needed |
| Email summarisation (bulk) | Gemini Flash | Many emails, cheap |
| Auto-reply template generation | GPT-4o-mini | Simple templates |

---

### Maintenance Superintendent
`/Users/aidengoode/Claude-Antigravity/Maintenance_Superintendent/`

| Task | Model | Reason |
|------|-------|--------|
| Maintenance schedule logic | Claude Sonnet | Standard code |
| Job card generation (bulk) | Gemini Flash | Templated, cheap |
| Equipment failure diagnosis | Claude Sonnet | Reasoning needed |
| Report writing / summaries | GPT-4o | Professional prose |
| Predictive maintenance models | Claude Sonnet | Code + analysis |
| Compliance documentation | GPT-4o | Document-quality output |
| Parts ordering automation | Claude Haiku | Simple rule-based logic |

---

### Agentic Trader (Personal)
`/Users/aidengoode/Claude-Antigravity/agentic-trader-personal/`

| Task | Model | Reason |
|------|-------|--------|
| Trading strategy design | Claude Opus | High-stakes, requires best reasoning |
| Risk management rules | Claude Opus | Accuracy critical — real money |
| Order execution code | Claude Sonnet | Standard code |
| Market sentiment analysis | Grok 2/3 | Real-time Twitter/X data |
| Crypto news monitoring | Grok 2/3 | Live data, x.ai trained on it |
| Backtesting logic | Claude Sonnet | Standard code |
| Trade log summarisation | Gemini Flash | Cheap batch summaries |
| Portfolio performance reports | GPT-4o | Good at financial prose |
| Exchange API integration | Claude Sonnet | Standard code |
| DEX scanning (from gem-scanner) | n8n + Grok | Realtime, automated |

---

## 3. Decision Flowchart

```
Task comes in
     │
     ▼
Is it trading logic or tax law? ──YES──► Claude Opus
     │
     ▼
Is it a security/architecture decision? ──YES──► Claude Opus
     │
     ▼
Is it writing code? ──YES──► Claude Sonnet (default)
     │
     ▼
Is it real-time market/social data? ──YES──► Grok via n8n
     │
     ▼
Is it a long document to summarise? ──YES──► Gemini 1.5 Pro (128k context)
     │
     ▼
Is it creative content (scripts/emails)? ──YES──► GPT-4o
     │
     ▼
Is it bulk/repetitive/classification? ──YES──► Gemini Flash (cheapest)
     │
     ▼
Default ──────────────────────────────────► Gemini Flash
```

---

## 4. n8n Router Workflow (Build in Automation-Stack)

Create a master "LLM Router" workflow in your n8n instance:

```
[Webhook: POST /llm-router]
  body: { task_type, prompt, context, project }
        │
        ▼
[Switch node on task_type]
  ├── "trading_strategy"    → Anthropic node (claude-opus-4-6)
  ├── "tax_law"             → Anthropic node (claude-opus-4-6)
  ├── "code_standard"       → Anthropic node (claude-sonnet-4-6)
  ├── "content_script"      → OpenAI node (gpt-4o)
  ├── "email_draft"         → OpenAI node (gpt-4o)
  ├── "bulk_classify"       → Google AI node (gemini-2.0-flash)
  ├── "long_doc_summary"    → Google AI node (gemini-1.5-pro)
  ├── "market_sentiment"    → HTTP Request → api.x.ai (grok-2)
  └── "default"             → Google AI node (gemini-2.0-flash)
        │
        ▼
[Return response to caller]
```

All your projects call this single endpoint — one place to update routing.

---

## 5. Create ~/.claude/.env.shared

Run this on your **local Mac** to create the file:

```bash
cat > ~/.claude/.env.shared << 'EOF'
# LLM API Keys — DO NOT COMMIT
# Last updated: 2026-03-16

# Anthropic (Claude) — console.anthropic.com
ANTHROPIC_API_KEY=sk-ant-

# OpenAI (GPT-4o, GPT-4o-mini) — platform.openai.com
OPENAI_API_KEY=sk-proj-

# Google (Gemini Pro + Flash) — aistudio.google.com
GOOGLE_API_KEY=AIza
GEMINI_API_KEY=AIza

# xAI (Grok 2/3) — console.x.ai
XAI_API_KEY=xai-

# GitHub — github.com/settings/tokens (scope: repo, workflow)
GITHUB_TOKEN=ghp_

# n8n
N8N_API_KEY=
N8N_WEBHOOK_URL=https://your-n8n-instance/webhook/llm-router

# Telegram (Agentic Trader + Crypto Scanner alerts)
TELEGRAM_BOT_TOKEN=
TELEGRAM_CHAT_ID=

# TikTok (Content Machine)
TIKTOK_CLIENT_KEY=
TIKTOK_CLIENT_SECRET=
TIKTOK_ACCESS_TOKEN=

# Gmail (Gmail Agent) — OAuth via Google Cloud Console
GMAIL_CLIENT_ID=
GMAIL_CLIENT_SECRET=
GMAIL_REFRESH_TOKEN=
EOF
```

Then add to your `~/.zshrc` (Mac default shell):
```bash
echo 'if [ -f ~/.claude/.env.shared ]; then set -a; source ~/.claude/.env.shared; set +a; fi' >> ~/.zshrc
source ~/.zshrc
```

Then update `~/.claude/settings.json`:
```json
{
  "model": "claude-sonnet-4-6",
  "env": {
    "ANTHROPIC_API_KEY": "${ANTHROPIC_API_KEY}",
    "OPENAI_API_KEY": "${OPENAI_API_KEY}",
    "GOOGLE_API_KEY": "${GOOGLE_API_KEY}",
    "XAI_API_KEY": "${XAI_API_KEY}",
    "GITHUB_TOKEN": "${GITHUB_TOKEN}",
    "N8N_WEBHOOK_URL": "${N8N_WEBHOOK_URL}"
  },
  "hooks": {
    "Stop": [{
      "matcher": "",
      "hooks": [{ "type": "command", "command": "~/.claude/stop-hook-git-check.sh" }]
    }]
  },
  "permissions": {
    "allow": ["Skill"]
  }
}
```

---

## 6. Sub-Agents to Create in ~/.claude/agents/

These route automatically to the right model within Claude Code:

### `trading-strategist.md` (Opus — high stakes)
```yaml
---
name: trading-strategist
model: claude-opus-4-6
description: Use for Agentic Trader strategy design, risk rules, and trading logic
---
You are an expert quantitative trading strategist...
```

### `tax-compliance.md` (Opus — accuracy critical)
```yaml
---
name: tax-compliance
model: claude-opus-4-6
description: Australian tax law interpretation and compliance logic for Tax Agent project
---
You are an Australian tax compliance expert...
```

### `content-writer.md` (Haiku — cheap creative)
```yaml
---
name: content-writer
model: claude-haiku-4-5-20251001
description: Fast content generation for GoMining reels and Content Machine
---
You write short-form social media content...
```

### `commit-writer.md` (Haiku — cheapest)
```yaml
---
name: commit-writer
model: claude-haiku-4-5-20251001
description: Generates concise git commit messages
---
Write a conventional commit message for the staged changes.
```

---

## 7. Monthly Token Budget Estimate

Assuming moderate daily usage across all projects:

| Spend | Model | Tasks |
|-------|-------|-------|
| ~$20/mo | Claude Sonnet | Daily coding across all projects |
| ~$10/mo | Claude Opus | Trading/tax critical decisions |
| ~$5/mo | GPT-4o | Content scripts, email drafts |
| ~$2/mo | Gemini Flash | Bulk classification, captions |
| ~$3/mo | Grok | Market sentiment for trader |
| **~$40/mo total** | | Across all 7 projects |

Without routing: using Opus for everything = **~$200-400/mo**
With routing: **~$40/mo** — ~85% cost reduction

# Global CLAUDE.md
# Deploy to: ~/.claude/CLAUDE.md on your local machine

---

# Claude Code — Global Configuration
# Account: infiniteyield.ai@gmail.com | GitHub: Infiniteyieldai
# Local machine: /Users/aidengoode/

## Identity & Context
You are working with Aiden Goode on a suite of AI automation and income-generation projects.
All projects live under `/Users/aidengoode/Claude-Antigravity/`.
Default to building autonomous, self-running systems that minimise manual intervention.

---

## Active Projects

| Project | Path | Purpose |
|---------|------|---------|
| **GoMining Reel Generation** | `~/Claude-Antigravity/GoMining Reel Generation/` | Automated video/reel content for GoMining crypto mining platform |
| **Content Machine** | `~/Claude-Antigravity/content-machine/` | TikTok video posting automation, social media pipeline |
| **Alpha Core** | `~/Claude-Antigravity/Alpha Core- Autonomous Income Ecosystem/` | Central orchestrator — autonomous income system across all projects |
| **Australian Tax Agent** | `~/Claude-Antigravity/Australian Tax Agent/` | AI tax agent for Australian tax compliance and optimisation |
| **Gmail Agent** | `~/Claude-Antigravity/Gmail Agent/` | Autonomous email management, triage, and response |
| **Maintenance Superintendent** | `~/Claude-Antigravity/Maintenance_Superintendent/` | AI maintenance planning and job management system |
| **Agentic Trader** | `~/Claude-Antigravity/agentic-trader-personal/` | Autonomous crypto/asset trading system |

---

## Model Selection Rules

**Use Claude Opus 4.6** (reserve for highest-stakes work only):
- Agentic Trader: trading logic, risk models, strategy code
- Alpha Core: architectural decisions, agent orchestration design
- Australian Tax Agent: tax law interpretation, compliance logic
- Security audits on any project
- Novel bugs that Sonnet can't crack

**Use Claude Sonnet 4.6** (default for all coding):
- All standard feature development across all projects
- Multi-step refactors, PR reviews
- Debugging known error types
- Integration work between projects

**Use Claude Haiku 4.5** (fast/cheap, delegate via sub-agent):
- Commit message generation
- Simple code formatting and linting fixes
- Quick lookups and explain-this-line requests
- Generating boilerplate from templates

**Route to external LLMs via n8n** (do NOT use Claude for these):
- GoMining / Content Machine: script writing, caption generation, hashtags → GPT-4o
- Gmail Agent: email drafting, reply suggestions → GPT-4o or Gemini Pro
- GoMining / Content Machine: bulk image/video description → Gemini Flash
- Agentic Trader: real-time market sentiment, crypto news → Grok
- Australian Tax Agent: summarising ATO documents → Gemini 1.5 Pro (long context)
- Any batch/classification task over 10 items → Gemini Flash

See `~/Claude-Antigravity/skill-builder/llm-routing-hierarchy.md` for full routing strategy.

---

## Tech Stack

- **Languages**: Go, TypeScript, Node.js (ESM) — NO Python
- **Scripts**: `.js` files with `import`/`export`, Node.js v24+
- **Infrastructure**: Docker + docker-compose, Cloudflare Tunnels
- **Automation**: n8n (self-hosted), PostgreSQL, Redis
- **Messaging**: Telegram Bot API, WhatsApp MCP
- **Social**: TikTok API (Content Machine), Gmail API (Gmail Agent)

## Coding Rules
- No Python — Node.js for all scripts
- ESM imports only (`import`/`export`), never `require()`
- Minimal abstractions — no over-engineering
- Docker-first for all services
- CLI tools: `gh`, `docker`, `npm`, `curl`, `jq`, `node`
- Commit and push before ending EVERY session (stop hook enforces this)

---

## GitHub Workflow
- Username: Infiniteyieldai
- Branch pattern: `claude/<task-slug>`
- Always use: `git push -u origin <branch>`
- Use `gh` CLI for all GitHub operations
- Never push to main directly

---

## Environment
- API keys: `~/.claude/.env.shared` (load in shell profile)
- n8n instance: [set your URL here]
- Anthropic API: https://api.anthropic.com
- Alpha Core is the orchestrator — changes to it affect all other projects

---

## Skills Available (~/.claude/skills/)
- `session-start-hook` — installs deps at web session start
- `skill-builder` — creates/edits/converts Claude Code skills

## Sub-Agents Available (~/.claude/agents/) — TO BE CREATED
- `trading-strategist` (Opus) — Agentic Trader logic
- `tax-compliance` (Opus) — Australian Tax Agent
- `content-writer` (Haiku) — GoMining/Content Machine copy
- `commit-writer` (Haiku) — fast commit messages

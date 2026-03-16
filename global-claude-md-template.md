# Global CLAUDE.md Template
# Copy this to: ~/.claude/CLAUDE.md

---

# Claude Code — Global Configuration
# Account: infiniteyield.ai@gmail.com | GitHub: Infiniteyieldai

## My Tech Stack
- **Languages**: Go, TypeScript, Node.js (ESM), JavaScript — NO Python
- **Scripts**: `.js` files only, `import`/`export` syntax, Node.js v24+
- **Infrastructure**: Docker + docker-compose, Cloudflare Tunnels
- **Automation**: n8n (self-hosted), PostgreSQL, Redis
- **Messaging**: Telegram Bot API, WhatsApp MCP
- **Blockchain**: DEXScreener API, EVM chains

## Active Projects
- `~/automation-stack` — n8n + WhatsApp + React dashboard (Docker)
- `~/crypto-gem-scanner` — DEX token scanner, Telegram alerts
- `~/skill-builder` — Claude Code skills framework
- `~/Content-Machine` — TikTok automation (WIP)

## Model Selection Rules
- **Use Opus** only for: security audits, architecture decisions, novel bugs
- **Use Sonnet** (default) for: all standard coding, multi-step tasks
- **Use Haiku** for: commit messages, simple lookups, quick formatting
- **Delegate to n8n** for: drafts, docs, bulk classification → use Gemini/GPT-4o there

## Coding Preferences
- No Python — use Node.js for scripts
- ESM imports (`import`/`export`), never CommonJS (`require`)
- Minimal abstractions — don't over-engineer
- Docker-first for services
- Commit and push before ending every session
- CLI tools preferred: `gh`, `docker`, `npm`, `curl`, `jq`

## GitHub Workflow
- Username: Infiniteyieldai
- Branch pattern: `claude/<task-slug>`
- Always commit with descriptive messages
- Use `gh` CLI for PR/issue operations
- GitHub MCP server: enabled (when configured)

## Skills Available
- `session-start-hook` — sets up web session dependencies
- `skill-builder` — creates/converts/edits Claude Code skills

## LLM Routing
See `~/skill-builder/llm-routing-hierarchy.md` for full routing strategy.
Route expensive work to Claude, cheap/bulk work to Gemini Flash or GPT-4o-mini via n8n.

## Environment
- API keys loaded from `~/.claude/.env.shared`
- n8n instance: [YOUR_N8N_URL]
- Anthropic API: https://api.anthropic.com

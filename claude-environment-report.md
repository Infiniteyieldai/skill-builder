# Claude Environment & GitHub Repository Report

**Account:** infiniteyield.ai@gmail.com
**GitHub:** Infiniteyieldai
**Generated:** 2026-03-16

---

## 1. Global CLAUDE.md Status

**STATUS: MISSING ❌**

No global `CLAUDE.md` file exists at either `~/.claude/CLAUDE.md` or `~/CLAUDE.md`.

This is the single most impactful gap in your setup. A global CLAUDE.md acts as persistent instructions injected into every Claude Code session, letting you define your preferences, workflows, tech stack defaults, and rules once across all projects.

**Recommended location:** `~/.claude/CLAUDE.md`

---

## 2. Current Claude Code Configuration

### Settings (`~/.claude/settings.json`)

```json
{
  "hooks": {
    "Stop": [git commit/push check]
  },
  "permissions": {
    "allow": ["Skill"]
  }
}
```

**What's configured:**
- ✅ **Stop hook** (`stop-hook-git-check.sh`) — enforces commit-and-push discipline at session end. Checks for uncommitted changes, untracked files, and unpushed commits. Works correctly.
- ✅ **Skill permissions** — Skills tool is globally allowed.

**What's missing:**
- ❌ No `SessionStart` hook configured (the skill exists but isn't wired into settings)
- ❌ No MCP servers configured
- ❌ No model preferences set
- ❌ No `env` section for injecting API keys

### Installed Skills (`~/.claude/skills/`)

| Skill | Status | Notes |
|-------|--------|-------|
| `session-start-hook` | ✅ Present | Helps create SessionStart hooks for web sessions |

**Gaps:** Only 1 skill installed. You have a full `skill-builder` repo with excellent templates — none of those skills have been built out yet.

### Plugin Blocklist (`~/.claude/plugins/blocklist.json`)

Two plugins are blocklisted:
- `code-review@claude-plugins-official` — blocked Feb 11, 2026 ("just-a-test")
- `fizz@testmkt-marketplace` — blocked Feb 12, 2026 ("security test")

These appear to be test entries. The blocklist is functioning but contains stale test data.

---

## 3. Available API Keys & Tokens

### Active in this Session

| Token | Source | Purpose | Scope |
|-------|--------|---------|-------|
| **Anthropic Session JWT** | `$CLAUDE_SESSION_INGRESS_TOKEN_FILE` | Authenticates Claude Code to Anthropic API | Short-lived (~4h), worker role |
| **CODESIGN_MCP_TOKEN** | Environment | Authenticates to local code-signing MCP server | Local only (port 39693) |

### API Endpoints

| Variable | Value | Purpose |
|----------|-------|---------|
| `ANTHROPIC_BASE_URL` | `https://api.anthropic.com` | Anthropic API endpoint |
| `CODESIGN_MCP_PORT` | `39693` | Local MCP code-signing server |
| `USE_SHTTP_MCP` | `true` | MCP transport enabled |

### What's Missing

- ❌ **No `ANTHROPIC_API_KEY`** — The session token is OAuth-based (for Claude Code CLI). To use the Anthropic API directly in your own apps/scripts, you need a separate API key from console.anthropic.com.
- ❌ **No GitHub token** — No `GITHUB_TOKEN` or `GH_TOKEN` in environment. This prevents `gh` CLI usage and GitHub API calls from scripts.
- ❌ **No n8n API key** — Despite n8n being central to your automation stack, no token configured.

### How to Use These APIs to Improve Your Systems

**Anthropic API (for your apps):**
```bash
# Get an API key from https://console.anthropic.com
# Add to ~/.claude/CLAUDE.md or project .env files
export ANTHROPIC_API_KEY=sk-ant-...
```
Use cases: claude-mem (session compression), ruflo (agent orchestration), direct API calls from scripts.

**CODESIGN_MCP (currently active):**
This provides code-signing capabilities via MCP at localhost:39693. Could be used to sign commits or artifacts automatically within Claude Code sessions.

**GitHub Token:**
```bash
# Add to settings.json env section:
# "env": { "GITHUB_TOKEN": "ghp_..." }
```
Would unlock `gh` CLI, GitHub MCP server, automated PR/issue management.

---

## 4. GitHub Repository Analysis

### Original Repos (Your Own Code)

| Repo | Status | Assessment |
|------|--------|------------|
| **Content-Machine** | ⚠️ Stub | 3 files, TikTok verification only — no actual code. Placeholder state. |
| **crypto-gem-scanner** | ✅ Active | Go+Python, n8n workflows, PostgreSQL, Telegram alerts. Most complete original project. |
| **Automation-stack** | ✅ Active | n8n + WhatsApp MCP + React dashboard + Docker. Self-hosted infrastructure stack. |

### Forked Repos (Collected but Not Customized)

| Repo | Upstream Purpose | Relevance to Your Stack |
|------|-----------------|------------------------|
| **claude-mem** | Session memory compression for Claude | HIGH — directly improves your Claude Code sessions |
| **ruflo** | Multi-agent swarm orchestration | HIGH — complements skill-builder |
| **skills** | Agent Skills framework | HIGH — the framework your skill-builder is based on |
| **skill-builder** | Skills builder (this repo) | HIGH — your primary Claude Code customization tool |
| **github-mcp-server** | GitHub's official MCP Server | HIGH — enables GitHub operations from Claude |
| **n8n** | Workflow automation platform | HIGH — core of your automation stack |
| **n8n-claude-code-guide** | n8n ↔ Claude Code integration | HIGH — bridges your two main systems |
| **awesome-claude-skills** | Curated skills collection | MEDIUM — resource for building more skills |
| **Google-Workspace-CLI** | Drive/Gmail/Calendar CLI in Rust | MEDIUM — useful if using Google Workspace |
| **n8n-workflows-directory** | Pre-built n8n workflows | MEDIUM — templates for automation-stack |
| **cc-nano-banana** | Image generation skill | LOW — novelty |
| **awesome-nano-banana-pro-prompts** | 8000+ prompts | LOW |
| **public-apis** | Free API directory | LOW — reference |
| **awesome-llm-apps-agents** | LLM app examples | LOW — reference |
| **compound-interest-site** | Educational finance site | LOW |

---

## 5. What's Working vs. Not Working

### ✅ Working Well

1. **Stop hook** — Git discipline enforced. Every session ends with a push-or-block check.
2. **skill-builder repo** — Excellent reference material, templates, and conversion guides. Well-structured.
3. **crypto-gem-scanner** — Most complete project. Go+n8n+PostgreSQL stack is coherent.
4. **Automation-stack** — Docker-based self-hosted infrastructure with WhatsApp and React.
5. **Skills framework understanding** — The SKILL.md system and conversion docs are thorough.

### ❌ Not Working / Gaps

1. **No global CLAUDE.md** — Every session starts from zero. You lose all context about your stack, preferences, and projects between sessions.
2. **Skills not built out** — You have the builder tool but only 1 skill deployed. The forks of `skills`, `awesome-claude-skills`, and `ruflo` suggest intent to build more.
3. **GitHub MCP not connected** — You forked `github-mcp-server` but it's not configured in Claude settings. You can't do GitHub operations from Claude Code.
4. **No SessionStart hook wired** — The `session-start-hook` skill exists but `SessionStart` isn't in `settings.json`. Dependencies won't auto-install.
5. **claude-mem not deployed** — The session memory compression fork is unused. This directly addresses your pain point of losing context between sessions.
6. **n8n ↔ Claude bridge unused** — You have `n8n-claude-code-guide` forked but no evidence of the integration being active.
7. **Content-Machine is empty** — TikTok automation tool has no code.
8. **Plugin blocklist has stale test data** — Should be cleaned up.
9. **No API key management strategy** — Tokens scattered across environment without a unified injection approach via settings.json.

---

## 6. Priority Recommendations

### Immediate (High Impact, Low Effort)

**1. Create `~/.claude/CLAUDE.md`**
This alone will transform your Claude sessions. Include:
- Your tech stack (Go, Node.js, TypeScript, n8n, Docker)
- Preferred patterns (no Python, ESM imports, Docker-compose)
- Your active projects and their locations
- GitHub username and workflow preferences
- Links to your skill-builder and key repos

**2. Add SessionStart hook to settings.json**
```json
"SessionStart": [{
  "matcher": "",
  "hooks": [{ "type": "command", "command": "~/.claude/session-start.sh" }]
}]
```

**3. Connect GitHub MCP server**
Configure `github-mcp-server` in Claude settings to enable native GitHub operations.

### Short-term (Medium Effort)

**4. Deploy claude-mem**
Your forked `claude-mem` compresses sessions and injects context. This directly solves the memory loss between sessions problem.

**5. Build 3-5 skills from your stack:**
- `managing-n8n-workflows` — CRUD operations on your n8n instance
- `deploying-docker-services` — for your automation-stack
- `scanning-crypto-gems` — wrapping your crypto-gem-scanner
- `managing-github-repos` — using gh CLI

**6. Add ANTHROPIC_API_KEY to settings.json env section**
Enables direct API usage in scripts and your Node.js tools.

### Longer-term

**7. Wire n8n ↔ Claude Code**
Use `n8n-claude-code-guide` to trigger Claude Code from n8n workflows and vice versa. This creates a full automation loop: n8n detects events → triggers Claude → Claude acts on code/GitHub.

**8. Build out Content-Machine**
The TikTok API verification files are there. Add actual video posting logic using Node.js + TikTok API.

**9. Consolidate ruflo**
Your `ruflo` fork (multi-agent orchestration) combined with your skills framework could enable autonomous multi-step workflows.

---

## 7. Suggested Global CLAUDE.md Structure

```markdown
# Claude Code Global Configuration

## About Me
- GitHub: Infiniteyieldai
- Email: infiniteyield.ai@gmail.com
- Primary projects: crypto-gem-scanner, Automation-stack, Content-Machine

## Tech Stack Preferences
- Languages: Go, TypeScript, Node.js (NOT Python)
- Scripts: .js files with ESM imports (import/export syntax)
- Node.js v24+ patterns
- Docker + docker-compose for infrastructure
- n8n for workflow automation
- PostgreSQL for databases

## Active Projects
- ~/automation-stack — Self-hosted n8n + WhatsApp + React
- ~/crypto-gem-scanner — DEXScreener alerts via Telegram
- ~/skill-builder — Claude Code skill templates

## Claude Code Preferences
- Always commit and push before ending sessions
- Use gh CLI for GitHub operations
- Prefer CLI tools over GUI approaches
- Skills location: ~/.claude/skills/

## Key Integrations
- n8n instance: [your URL]
- Telegram bot: [for crypto alerts]
- GitHub MCP server: enabled
```

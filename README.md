# 🤖 DevFlow AI — Claude-Powered Pull Request Automation

A fully-integrated MCP (Model Context Protocol) server that turns pull request chaos into team-wide clarity.  
This agent analyzes code changes, monitors CI/CD pipelines, and sends real-time Slack updates — all powered by Claude Code and the MCP standard.

---

## 🚀 Features

- 🧠 **AI-Powered PR Suggestions**  
  Uses Claude Code to analyze git diffs and suggest the best PR templates with clear, actionable descriptions.

- 🧪 **CI/CD Monitoring (GitHub Actions)**  
  Tracks workflows via GitHub webhooks, highlights failures, and summarizes deployment outcomes.

- 📣 **Slack Team Notifications**  
  Sends professionally formatted alerts and summaries using Slack markdown and team routing logic.

- 💠 **Plug-and-Play Tools & Prompts**  
  Designed using MCP primitives:
  - **Tools** for gathering structured data (git, CI, Slack)
  - **Prompts** for guiding Claude through consistent workflows
  - **Resources** for project context and team conventions

---

## 🔍 Why This Matters

Development teams often struggle with:

- Poor pull request descriptions
- Missed CI/CD failures
- Communication silos across teams

This project addresses all three. It automates tedious dev workflows while preserving human context and clarity — so your team ships confidently, communicates clearly, and spends less time deciphering PRs or tracking down failures.

---

## 📆 Technologies Used

- **[Claude Code](https://console.anthropic.com/)** + **Model Context Protocol (MCP)**  
- **Python 3.11+**
- **FastMCP** — for MCP server scaffolding  
- **GitHub Webhooks** — via `aiohttp` and Cloudflare Tunnel  
- **Slack Webhooks** — with secure environment-based secrets  
- **Git CLI** — for accurate diff and commit history analysis  

---

## 🧹 Architecture Overview

```plaintext
Developer → Git Push → GitHub → Webhook Server
                                  ↓
      Claude Code ← MCP Server ← github_events.json
            ↓                     
    Slack Notification
```

| Component           | Description                                      |
|--------------------|--------------------------------------------------|
| `server.py`         | The main MCP server with all Tools & Prompts     |
| `webhook_server.py` | Listens for GitHub Actions events                |
| `github_events.json`| Stores the last 100 CI/CD events locally         |
| Templates    | Structured natural language report templates for Claude|

---

## ⚙️ Getting Started

### 🔧 Prerequisites
- Python 3.11+
- [Claude Code CLI](https://docs.anthropic.com/claude/docs/claude-code)
- GitHub repo with Actions enabled
- Slack workspace with [incoming webhooks](https://api.slack.com/messaging/webhooks)
- `uv` package manager (or use `pip`/`venv` as needed)

### 🧲 Setup & Run

```bash
# Clone the repo
git clone https://github.com/your-username/devflow-ai.git
cd devflow-ai

# Install dependencies
uv venv .venv
source .venv/bin/activate
uv sync

# Start webhook listener 
python webhook_server.py

# Start MCP server (in separate terminal)
python server.py

# Tunnel webhook to GitHub
cloudflared tunnel --url http://localhost:8080
```

### 🔐 Environment Configuration
```bash
# Set your Slack Webhook URL securely
export SLACK_WEBHOOK_URL="https://hooks.slack.com/services/XXXX/YYYY/ZZZZ"
```

---

## ✅ Example Usage

Ask Claude:

> _"Can you analyze my code changes and suggest a PR template?"_  
> _"Summarize our recent deployment and notify the team in Slack."_  
> _"Troubleshoot the failed `ci-lint` GitHub Action."_

Claude will:

- Call your registered MCP Tools
- Use project Resources for context
- Apply your Prompts to return consistent reports

---

## 📈 Impact

| Before | After |
|--------|-------|
| PRs with titles like `"fix"` and `"update stuff"` | Auto-generated summaries with correct templates and security checks |
| Developers manually checking Actions tab | Real-time notifications with status breakdowns |
| Missed cross-team updates | Slack alerts aligned with team preferences |
| Repeated debugging of known issues | Prompt-based guidance from recent history |

---

## 💪 Testing

Run unit tests:
```bash
uv run pytest tests/unit/
```

Run integration tests:
```bash
uv run pytest tests/integration/
```

Use `manual_test.md` to simulate GitHub events locally via `curl`.

---

## 📋 Learn More

- [MCP Documentation]([https://huggingface.co/docs/mcp](https://modelcontextprotocol.io/docs/getting-started/intro))
- [Claude Code Guide](https://docs.anthropic.com/claude/docs/claude-code)
- [Slack Message Formatting](https://api.slack.com/reference/surfaces/formatting)
- [GitHub Webhooks Guide](https://docs.github.com/en/webhooks)

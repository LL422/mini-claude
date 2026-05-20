# Mini Claude — AI Coding Assistant

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)](#)
[![Lines of Code](https://img.shields.io/badge/~4200_lines-python-green)](#)

> A lightweight AI coding assistant that works in your terminal. Read code, write files, search the web, run commands — all through natural language.

Built on [claude-code-from-scratch](https://github.com/Windy3f3f3f3f/claude-code-from-scratch) (MIT), with added error self-healing, web search, streaming shell, multi-model support, conversation export, and path-level permissions.

---

## Features

Chat with AI in your terminal. It can:

- **Read & write code** — read files, create files, make precise edits
- **Search your project** — find files by glob pattern, search code with regex
- **Run commands** — execute tests, install dependencies, git operations with real-time output
- **Search the web** — look up current docs, error solutions, API references
- **Self-heal errors** — when a tool fails, the AI analyzes the error and tries a different approach
- **Export conversations** — save chats as Markdown notes
- **Remember preferences** — cross-session memory for your coding habits
- **Delegate to sub-agents** — spawn isolated agents for search, planning, and code review
- **Connect external tools** — MCP protocol for third-party tool servers

---

## Quick Start

Requires Python 3.11+.

```bash
git clone https://github.com/LL422/mini-claude.git
cd mini-claude/python
pip install -e .
```

### API Configuration

Three backend options, pick one:

```bash
# DeepSeek (Anthropic-compatible, affordable)
export DEEPSEEK_API_KEY="sk-xxx"
mini-claude-py --provider deepseek

# Ollama local model (free, requires Ollama installed)
mini-claude-py --provider ollama --model ollama/qwen2.5

# Anthropic Claude (native)
export ANTHROPIC_API_KEY="sk-ant-xxx"
mini-claude-py

# OpenAI-compatible (Qwen, GPT, etc.)
export OPENAI_API_KEY="sk-xxx"
export OPENAI_BASE_URL="https://api.openai.com/v1"
mini-claude-py --api-base $OPENAI_BASE_URL
```

### Run

```bash
mini-claude-py                    # Interactive REPL mode
mini-claude-py "fix the bug"      # One-shot mode
mini-claude-py --resume           # Resume last session
mini-claude-py --yolo             # Skip safety confirmations
mini-claude-py --plan             # Read-only plan mode
mini-claude-py --max-cost 0.5     # Cost limit in USD
mini-claude-py --max-turns 20     # Max agentic turns
mini-claude-py --thinking         # Enable extended thinking
mini-claude-py --model gpt-4o     # Specify model
```

---

## REPL Commands

| Command | Action |
|---------|--------|
| `/clear` | Clear conversation history |
| `/plan` | Toggle plan mode (read-only) |
| `/cost` | Show token usage and cost |
| `/compact` | Manually compact conversation |
| `/memory` | List saved memories |
| `/skills` | List available skills |
| `/export [filename]` | Export conversation as Markdown |
| `/<skill>` | Invoke a skill (e.g. `/commit`) |

---

## Tools

14 tools available to the AI:

| Tool | Purpose |
|------|---------|
| `read_file` | Read file contents |
| `write_file` | Create or overwrite files |
| `edit_file` | Precise string replacement in files |
| `list_files` | Find files by glob pattern |
| `grep_search` | Regex search in code |
| `run_shell` | Execute shell commands (streaming output) ✨ |
| `web_search` | Search the web via DuckDuckGo ✨ |
| `web_fetch` | Fetch and parse URL content |
| `agent` | Spawn sub-agents for isolated tasks |
| `skill` | Invoke registered skills |
| `enter_plan_mode` | Enter read-only planning |
| `exit_plan_mode` | Exit planning with approval workflow |
| `tool_search` | Search for available deferred tools |

✨ = new or enhanced in this fork

---

## Security

| Feature | Description |
|---------|-------------|
| 5 permission modes | default / plan / acceptEdits / bypassPermissions / dontAsk |
| Declarative rules | allow/deny rules in `.claude/settings.json` |
| Path-level permissions ✨ | Glob-based file access control (e.g. `read_file(config/**): deny`) |
| Dangerous command detection | 16 regex patterns covering `rm`, `sudo`, `git push --force`, etc. |
| Read-before-edit guard | Files must be read before editing |

---

## Comparison with Claude Code

| Aspect | Claude Code | Mini Claude |
|--------|------------|-------------|
| Purpose | Production agent | Lightweight assistant |
| Tools | 66+ | 14 (incl. web_search) |
| Model support | Anthropic | Anthropic / DeepSeek / Ollama / OpenAI ✨ |
| Error handling | 7 recovery strategies | Self-healing + exponential backoff ✨ |
| Shell output | Wait for completion | Real-time streaming ✨ |
| Context | 4-tier compression | 4-tier compression + large result persistence |
| Permissions | 7-layer + AST | 5 modes + rules + path-level glob ✨ |
| Memory | 4 types + semantic recall | 4 types + semantic recall + async prefetch |
| Code size | 500k+ | ~4200 lines (Python) |

---

## Project Structure

```
python/
├── mini_claude/
│   ├── agent.py         # Agent loop, streaming, compression, Plan Mode
│   ├── tools.py         # 14 tools, permissions, danger detection
│   ├── __main__.py      # CLI entry, REPL, argument parsing
│   ├── prompt.py        # System prompt builder
│   ├── memory.py        # Memory system (semantic recall)
│   ├── skills.py        # Skills system (inline/fork)
│   ├── subagent.py      # Sub-agents (3 built-in + custom)
│   ├── mcp_client.py    # MCP JSON-RPC client
│   ├── ui.py            # Terminal UI
│   ├── session.py       # Session persistence
│   └── frontmatter.py   # YAML frontmatter parser
└── pyproject.toml
```

---

## License

MIT — see [LICENSE](./LICENSE)

## Credits

Built on [claude-code-from-scratch](https://github.com/Windy3f3f3f3f/claude-code-from-scratch) by [Windy3f3f3f3f](https://github.com/Windy3f3f3f3f) (MIT License). Original tutorial available at [claude-code-from-scratch docs](https://windy3f3f3f3f.github.io/claude-code-from-scratch/).

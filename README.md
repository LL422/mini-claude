# Mini Claude — AI Coding Assistant

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)](#)
[![Lines of Code](https://img.shields.io/badge/~4200_lines-python-green)](#)

> 一个轻量级 AI 编程助手，在终端里用自然语言完成编码任务——读代码、写文件、搜资料、跑命令。

基于 [claude-code-from-scratch](https://github.com/Windy3f3f3f3f/claude-code-from-scratch) (MIT) 二次开发，新增了错误自修复、网页搜索、流式 Shell、多模型支持、对话导出和路径级权限等实用能力。

---

## 能做什么

在终端里跟 AI 对话，它可以：

- **读写代码** — 读文件、写文件、精确编辑
- **搜索项目** — 按文件名或正则搜代码
- **执行命令** — 跑测试、安装依赖、git 操作，输出实时显示
- **上网搜索** — 查最新文档、报错解决方案
- **自我纠错** — 工具出错不崩，AI 自己分析原因并修正
- **导出对话** — 把好的交流保存成 Markdown 笔记
- **记住偏好** — 跨会话记住你的习惯和项目约定
- **派分身** — 启动子代理独立完成搜索、规划、审查等子任务
- **接入外部工具** — 通过 MCP 协议连接第三方工具服务器

---

## 快速开始

需要 Python 3.11+。

```bash
git clone https://github.com/LL422/mini-claude.git
cd mini-claude/python
pip install -e .
```

### 配置 API Key

支持三种后端，任选一种：

```bash
# DeepSeek（便宜，推荐）
export DEEPSEEK_API_KEY="sk-xxx"
mini-claude-py --provider deepseek

# Ollama 本地模型（免费，需先装 Ollama）
mini-claude-py --provider ollama --model ollama/qwen2.5

# Anthropic Claude（原生）
export ANTHROPIC_API_KEY="sk-ant-xxx"
mini-claude-py

# OpenAI 兼容（通义千问、GPT 等）
export OPENAI_API_KEY="sk-xxx"
export OPENAI_BASE_URL="https://api.openai.com/v1"
mini-claude-py --api-base $OPENAI_BASE_URL
```

### 运行

```bash
mini-claude-py                    # 交互式 REPL 模式
mini-claude-py "帮我修复 bug"      # 一问一答模式
mini-claude-py --resume           # 恢复上次对话
mini-claude-py --yolo             # 跳过安全确认
mini-claude-py --plan             # 只读规划模式
mini-claude-py --max-cost 0.5     # 最多花 $0.5
mini-claude-py --max-turns 20     # 最多 20 轮
mini-claude-py --thinking         # 启用深度思考
mini-claude-py --model gpt-4o     # 指定模型
```

---

## REPL 命令

在交互模式下可以用的命令：

| 命令 | 功能 |
|------|------|
| `/clear` | 清空对话历史 |
| `/plan` | 进入/退出只读规划模式 |
| `/cost` | 查看 token 用量和费用 |
| `/compact` | 手动压缩对话（上下文太长时） |
| `/memory` | 列出已保存的记忆 |
| `/skills` | 列出可用的技能 |
| `/export [文件名]` | 导出对话为 Markdown 文件 |
| `/<技能名>` | 调用技能（如 `/commit`） |

---

## 工具列表

AI 可以调用的 14 个工具：

| 工具 | 做什么 |
|------|--------|
| `read_file` | 读文件 |
| `write_file` | 写文件（自动建目录） |
| `edit_file` | 精确编辑文件中的某段代码 |
| `list_files` | 按 glob 模式搜文件 |
| `grep_search` | 正则搜索代码内容 |
| `run_shell` | 执行终端命令（实时输出）✨ |
| `web_search` | DuckDuckGo 搜索互联网 ✨ |
| `web_fetch` | 抓取指定 URL 的内容 |
| `agent` | 派子代理独立完成任务 |
| `skill` | 调用已注册的技能 |
| `enter_plan_mode` | 进入只读规划模式 |
| `exit_plan_mode` | 退出规划模式并提交计划审批 |
| `tool_search` | 搜索可用的延迟加载工具 |

✨ = 二次开发新增或增强

---

## 安全机制

| 特性 | 说明 |
|------|------|
| 5 种权限模式 | default / plan / acceptEdits / bypassPermissions / dontAsk |
| 声明式规则 | `.claude/settings.json` 配置 allow/deny 规则 |
| 路径级权限 ✨ | glob 模式限制 AI 的文件访问范围（如 `read_file(config/**): deny`） |
| 危险命令检测 | 16 个正则覆盖 `rm`、`sudo`、`git push --force` 等 |
| 先读后写保护 | 编辑文件前必须先读过 |

---

## 与 Claude Code 的对比

| 维度 | Claude Code | Mini Claude |
|------|------------|-------------|
| 定位 | 生产级编程智能体 | 轻量级 AI 编程助手 |
| 工具数量 | 66+ | 14（含 web_search） |
| 多模型支持 | Anthropic | Anthropic / DeepSeek / Ollama / OpenAI ✨ |
| 错误处理 | 7 种恢复策略 | 错误自修复 + 指数退避重试 ✨ |
| Shell 输出 | 等待完成 | 实时流式输出 ✨ |
| 上下文管理 | 4 级压缩 | 4 级压缩 + 大结果持久化 |
| 权限系统 | 7 层 + AST 分析 | 5 模式 + 规则 + 路径级权限 ✨ |
| 记忆系统 | 4 类型 + 语义召回 | 4 类型 + 语义召回 + 异步预取 |
| 代码量 | 50 万+ | ~4200 行（Python） |

---

## 项目结构

```
python/
├── mini_claude/
│   ├── agent.py         # Agent 循环、流式、压缩、Plan Mode
│   ├── tools.py         # 14 个工具、权限、危险检测
│   ├── __main__.py      # CLI 入口、REPL、参数解析
│   ├── prompt.py        # 系统提示词构建
│   ├── memory.py        # 记忆系统（语义召回）
│   ├── skills.py        # 技能系统（inline/fork）
│   ├── subagent.py      # 子代理（3 内置 + 自定义）
│   ├── mcp_client.py    # MCP JSON-RPC 客户端
│   ├── ui.py            # 终端 UI
│   ├── session.py       # 会话持久化
│   └── frontmatter.py   # YAML frontmatter 解析
└── pyproject.toml
```

---

## License

MIT — 详见 [LICENSE](./LICENSE)

## 致谢

本项目基于 [claude-code-from-scratch](https://github.com/Windy3f3f3f3f/claude-code-from-scratch) (MIT License)，原始教程可在 [这里](https://windy3f3f3f3f.github.io/claude-code-from-scratch/) 阅读。

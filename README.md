# Epsimo Agent Framework

> **Beta Release (v0.3.3)** — Build AI-powered applications with agents, persistent threads, and Virtual Database state management.

The Epsimo Agent Framework provides a unified **CLI**, **Python SDK**, and **React UI Kit** for building AI applications with:

- 🤖 Multi-agent orchestration with streaming responses
- 💾 Virtual Database — thread-based persistent state (no DB server needed)
- 🖥️ Interactive TUI dashboard with live data
- 🎨 Pre-built React chat components
- 🔌 Extensible tool library (web search, retrieval, task management)
- ⚡ One-command project scaffolding with Next.js

**API Base URL:** `https://backend.epsimoagents.com`
**Web App:** [app.epsimoagents.com](https://app.epsimoagents.com)

This repository holds the `epsimo-agents` agent skill (in [`skills/epsimo-agents/`](skills/epsimo-agents/SKILL.md)) and the source of the `epsimo` CLI, Python SDK and React UI Kit for [EpsimoAgents](https://app.epsimoagents.com). It is maintained by Epsimo AI.

## Install the agent skill

```bash
npx skills add epsimoai/epsimo-agents
```

The `skills` CLI needs Node.js 22.20 or later. The skill runs the `epsimo` command, which needs Python 3.8 or later. Install the command with one of the options under [Installation](#-installation): the one-line installer, the `epsimo-cli` npm package, or from source.

Supported agent tools:

| Agent tool | Command | Project path | Global path (with `-g`) |
| --- | --- | --- | --- |
| Claude Code | `npx skills add epsimoai/epsimo-agents -a claude-code` | `.claude/skills/epsimo-agents/` | `~/.claude/skills/epsimo-agents/` |
| OpenAI Codex CLI | `npx skills add epsimoai/epsimo-agents -a codex` | `.agents/skills/epsimo-agents/` | `~/.agents/skills/epsimo-agents/` |
| Cursor | `npx skills add epsimoai/epsimo-agents -a cursor` | `.agents/skills/epsimo-agents/` | `~/.cursor/skills/epsimo-agents/` |
| Kiro (IDE and CLI) | `npx skills add epsimoai/epsimo-agents -a kiro-cli` | `.kiro/skills/epsimo-agents/` | `~/.kiro/skills/epsimo-agents/` |
| Other tools | `npx skills add epsimoai/epsimo-agents`, then pick the tool | per tool | per tool |

In the Kiro IDE you can also import the skill from GitHub with the URL `https://github.com/epsimoai/epsimo-agents/tree/main/skills/epsimo-agents`. See the [skills CLI agent list](https://github.com/vercel-labs/skills/blob/main/docs/agents.md) for other tools.

### Former skill name

This skill was previously published as `epsimo-agent`. If you installed it under that name, remove it and install the new one:

```bash
npx skills remove epsimo-agent
npx skills add epsimoai/epsimo-agents
```

### Credentials


| Variable | Status | Where to get it |
| --- | --- | --- |
| `EPSIMO_API_KEY` | Recommended | Read only by the Python SDK (`EpsimoClient()`), which sends it as an `Authorization: Bearer` token; if it is not set, the SDK uses the token cached in `~/.epsimo_token`. The `epsimo` CLI and the helper scripts do not read it |
| `EPSIMO_EMAIL`, `EPSIMO_PASSWORD` | Optional alternative to `EPSIMO_API_KEY` | Your EpsimoAgents account email and password. `epsimo auth` uses them to log in and caches a token in `~/.epsimo_token` |
| `EPSIMO_API_URL` | Optional | API base URL; set it only to target another API |

You need an EpsimoAgents account. The `epsimo` CLI and the helper scripts sign in with `EPSIMO_EMAIL` and `EPSIMO_PASSWORD` (`epsimo auth`, or `python3 scripts/auth.py login` from the skill directory). Keep keys and passwords out of chats, prompts and commits.

### Sample prompt

> List my EpsimoAgents projects and the assistants in the first project, then start a new thread with one of those assistants and show its Virtual Database state.

[EpsimoAgents homepage](https://app.epsimoagents.com)

---

## Table of Contents

- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [CLI Reference](#-cli-reference)
- [Interactive TUI Dashboard](#-interactive-tui-dashboard)
- [Python SDK](#-python-sdk)
- [React UI Kit](#-react-ui-kit)
- [Virtual Database](#-virtual-database)
- [Tool Library](#-tool-library)
- [Authentication & Security](#-authentication--security)
- [Project Structure](#-project-structure)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [License](#-license)

---

## 📦 Installation

### For AI Coding Agents (Recommended)

Install as a skill for Claude Code, Cursor, Cline, Windsurf, and 30+ other AI coding agents:

```bash
npx skills add epsimoai/epsimo-agents
```

### npm (Global)

```bash
npm install -g epsimo-cli
```

### Python (from source)

```bash
git clone https://github.com/epsimoai/epsimo-agents.git
cd epsimo-agents
pip install -e .
```

### One-line installer (macOS / Linux)

```bash
curl -fsSL https://raw.githubusercontent.com/epsimoai/epsimo-agents/main/install.sh | bash
```

**Requirements:** Python 3.8+, Node.js 14+ (for npm install)

---

## 🚀 Quick Start

```bash
# 1. Authenticate
epsimo auth

# 2. Scaffold a new Next.js project
epsimo create "My AI App"

# 3. Link to platform and deploy
cd my-ai-app
epsimo init
epsimo deploy

# 4. Chat with your assistant
epsimo chat

# 5. Or launch the interactive dashboard
epsimo tui
```

See [docs/getting-started.md](docs/getting-started.md) for a detailed walkthrough.

---

## 🛠️ CLI Reference

### Core Commands

| Command | Description |
|---------|-------------|
| `epsimo auth` | Interactive login (email/password) |
| `epsimo auth --force` | Re-authenticate even if already logged in |
| `epsimo whoami` | Show current user email and thread usage |
| `epsimo create <name>` | Scaffold a new Next.js app with Epsimo UI Kit |
| `epsimo init` | Link current directory to a new Epsimo project |
| `epsimo init --name "Bot"` | Use a custom project name |
| `epsimo deploy` | Sync `epsimo.yaml` assistants to the platform |

### Chat & Interaction

| Command | Description |
|---------|-------------|
| `epsimo chat` | Interactive chat with real-time tool call visibility |
| `epsimo chat --tools search_tavily,ddg_search` | Chat with specific tools enabled |
| `epsimo run` | Simple interactive chat (auto-selects project/assistant) |
| `epsimo run --project-id <ID> --assistant-id <ID>` | Chat with a specific assistant |
| `epsimo tui` | Launch the interactive terminal dashboard |

### Resource Management

| Command | Description |
|---------|-------------|
| `epsimo projects` | List all projects |
| `epsimo projects --json` | Output as JSON |
| `epsimo assistants --project-id <ID>` | List assistants in a project |
| `epsimo assistants --project-id <ID> --json` | Output as JSON |
| `epsimo threads --project-id <ID>` | List threads in a project |

### Virtual Database

| Command | Description |
|---------|-------------|
| `epsimo db query --project-id <ID> --thread-id <ID>` | View all stored key-value pairs |
| `epsimo db get --project-id <ID> --thread-id <ID> --key <K>` | Get a specific key |
| `epsimo db set --project-id <ID> --thread-id <ID> --key <K> --value <V>` | Set a key-value pair |

### Tools & Search

| Command | Description |
|---------|-------------|
| `epsimo tools` | List available tools |
| `epsimo tools --json` | Output tool list as JSON |
| `epsimo tools health <type>` | Health-check a tool (e.g. `ddg_search`) |
| `epsimo search "query"` | Web search via assistant |
| `epsimo search "query" --tool ddg_search` | Search with a specific provider |
| `epsimo exec "code"` | Send code to assistant for execution |
| `epsimo exec --file script.py` | Execute a file |

### Credits & Billing

| Command | Description |
|---------|-------------|
| `epsimo credits balance` | Check thread usage and remaining balance |
| `epsimo credits buy --quantity 1000` | Generate a Stripe checkout URL |

**Pricing:** <500 threads: €0.10/ea · 500–999: €0.09/ea · 1000+: €0.08/ea

---

## 🖥️ Interactive TUI Dashboard

Launch with `epsimo tui` for a full terminal dashboard with live data:

```
┌─────────────────────────────────────────────────────────┐
│  EPSIMO TUI                                             │
│  [1] Status  [2] Projects  [3] Assistants               │
│  [4] Threads [5] DB        [6] Tools                    │
├─────────────────────────────────────────────────────────┤
│  Navigate: Arrow keys / Number keys                     │
│  [P] Switch project  [R] Refresh  [Enter] Drill in     │
│  [Q] Quit                                               │
└─────────────────────────────────────────────────────────┘
```

**Features:**
- 6 tabs: Status, Projects, Assistants, Threads, Virtual DB, Tools
- Live animated spinners during data loading
- Slash commands inside chat: `/help`, `/status`, `/credits`, `/db`, `/tools`, `/threads`, `/switch`, `/create`, `/buy`, `/clear`
- Real-time streaming with tool call visualization

See [docs/tui-guide.md](docs/tui-guide.md) for the full TUI reference.

---

## 📚 Python SDK

```python
from epsimo import EpsimoClient

# Initialize (uses ~/.epsimo_token or EPSIMO_API_KEY env var)
client = EpsimoClient()

# Or with explicit token
client = EpsimoClient(api_key="your-jwt-token")
```

### Resource Clients

| Resource | Access | Purpose |
|----------|--------|---------|
| Projects | `client.projects` | Top-level containers |
| Assistants | `client.assistants` | AI agents with instructions & tools |
| Threads | `client.threads` | Persistent conversations |
| Files | `client.files` | Document uploads for retrieval |
| Credits | `client.credits` | Billing & usage |
| Database | `client.db` | Virtual DB access |

### Examples

```python
# List projects
projects = client.projects.list()

# Create a project
project = client.projects.create(name="My Project", description="...")

# Create an assistant
assistant = client.assistants.create(
    project_id="proj_abc",
    name="Support Agent",
    model="gpt-4o",
    instructions="You are a helpful support agent.",
    tools=[{"type": "search_tavily"}]
)

# Stream a conversation
for chunk in client.threads.run_stream(
    project_id="proj_abc",
    thread_id="thread_123",
    assistant_id="asst_xyz",
    message="Hello!"
):
    print(chunk, end="", flush=True)

# Virtual Database
db_state = client.db.get_all("proj_abc", "thread_123")
client.db.set("proj_abc", "thread_123", "status", "active")
```

---

## 🎨 React UI Kit

### ThreadChat Component

```tsx
import { ThreadChat } from "@/components/epsimo";

export default function App() {
  return (
    <ThreadChat
      assistantId="asst_xyz"
      projectId="proj_abc"
      placeholder="Ask me anything..."
      theme="dark"
    />
  );
}
```

### useChat Hook (Headless)

```tsx
import { useChat } from "@/hooks/epsimo";

export default function CustomChat() {
  const { messages, sendMessage, isLoading, error } = useChat({
    projectId: "proj_abc",
    threadId: "thread_123",
    assistantId: "asst_xyz"
  });

  return (
    <div>
      {messages.map(msg => (
        <div key={msg.id} className={msg.role}>{msg.content}</div>
      ))}
      <button onClick={() => sendMessage("Hello")} disabled={isLoading}>
        {isLoading ? "Sending..." : "Send"}
      </button>
      {error && <div className="error">{error}</div>}
    </div>
  );
}
```

**Features:** Real-time streaming, tool call visualization, message history, dark/light themes, mobile responsive.

---

## 💾 Virtual Database

Threads serve as persistent, structured storage — no separate database needed.

### How It Works

1. **Agent writes** using the `update_database` tool during conversation
2. **Data persists** in thread state, partitioned per thread
3. **Query anywhere** — from SDK, CLI, or frontend

```python
# SDK: read
prefs = client.db.get("proj_abc", "thread_123", "user_preferences")

# SDK: write
client.db.set("proj_abc", "thread_123", "status", "active")
```

```bash
# CLI: read
epsimo db query --project-id proj_abc --thread-id thread_123

# CLI: write
epsimo db set --project-id proj_abc --thread-id thread_123 \
  --key "status" --value '"completed"'
```

**Benefits:** Zero configuration · Data partitioned by conversation · Agent always aware of its state · Queryable from any client

See [docs/virtual_db_guide.md](docs/virtual_db_guide.md) for the full guide.

---

## 🔧 Tool Library

Reusable tool schemas in `epsimo/tools/library.yaml`:

| Tool | Type | Description |
|------|------|-------------|
| `database_sync` | function | Persist structured JSON to thread state |
| `web_search_tavily` | search_tavily | Advanced web search with source attribution |
| `web_search_ddg` | ddg_search | Fast DuckDuckGo search |
| `retrieval_optimized` | retrieval | Document search in uploaded files |
| `task_management` | function | Track and update user tasks |

### Using Tools in epsimo.yaml

```yaml
assistants:
  - name: "Research Assistant"
    model: "gpt-4o"
    instructions: "You help with research tasks."
    tools:
      - type: search_tavily
      - type: function
        name: update_database
        description: "Save research findings"
        parameters:
          type: object
          properties:
            key: { type: string }
            value: { type: object }
          required: ["key", "value"]
```

See [docs/configuration.md](docs/configuration.md) for the full `epsimo.yaml` reference.

---

## 🔐 Authentication & Security

### Login

```bash
epsimo auth                    # Interactive prompt
epsimo auth --force            # Force re-authentication
```

Or set environment variables:

```bash
export EPSIMO_EMAIL=your@email.com
export EPSIMO_PASSWORD=your-password
```

### Token Storage

Tokens are stored in `~/.epsimo_token` as JSON and expire after **1 hour**. The CLI auto-refreshes from env vars when expired.

### Security Best Practices

- Never commit `.epsimo_token` or `.env` files
- Use `EPSIMO_API_KEY` environment variable in production
- Use project-specific tokens for multi-tenant apps
- Rotate tokens regularly

---

## 📁 Project Structure

```
epsimo-agents/
├── epsimo/
│   ├── cli.py              # Main CLI (argparse)
│   ├── cli_smart.py        # Smart commands: chat, exec, search, tools
│   ├── tui.py              # Interactive TUI dashboard
│   ├── client.py           # SDK client (EpsimoClient)
│   ├── auth.py             # Authentication & token management
│   ├── resources/          # Resource-specific SDK clients
│   │   ├── projects.py
│   │   ├── assistants.py
│   │   ├── threads.py
│   │   ├── files.py
│   │   ├── credits.py
│   │   └── db.py
│   ├── tools/
│   │   └── library.yaml    # Reusable tool schemas
│   └── templates/          # Project scaffolding templates
│       ├── next-mvp/       # Next.js starter template
│       └── components/     # React UI Kit components
├── skills/
│   └── epsimo-agents/      # The agent skill (installed by `npx skills add`)
│       ├── SKILL.md        # Skill documentation
│       ├── scripts/        # Helper & test scripts
│       └── references/     # API reference documentation
├── docs/                   # Guides and tutorials
├── install.sh              # macOS/Linux installer
├── install.ps1             # Windows installer
├── epsimo.yaml             # Example configuration
├── pyproject.toml          # Python package config
├── package.json            # npm package config
├── CONTRIBUTING.md         # Contribution guidelines
├── CHANGELOG.md            # Version history
└── LICENSE                 # MIT License
```

---

## 📖 Documentation

| Document | Description |
|----------|-------------|
| [Getting Started](docs/getting-started.md) | Step-by-step tutorial for new users |
| [TUI Guide](docs/tui-guide.md) | Interactive dashboard reference |
| [Virtual DB Guide](docs/virtual_db_guide.md) | Thread-based storage patterns |
| [Configuration Reference](docs/configuration.md) | `epsimo.yaml` schema and options |
| [API Reference](skills/epsimo-agents/references/api_reference.md) | Complete REST API documentation |
| [SKILL.md](skills/epsimo-agents/SKILL.md) | Full skill documentation for AI agents |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to contribute |
| [CHANGELOG.md](CHANGELOG.md) | Version history |

---

## 🤝 Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🔗 Links

- **GitHub:** [github.com/epsimoai/epsimo-agents](https://github.com/epsimoai/epsimo-agents)
- **npm:** [npmjs.com/package/epsimo-cli](https://www.npmjs.com/package/epsimo-cli)
- **Web App:** [app.epsimoagents.com](https://app.epsimoagents.com)
- **API:** `https://backend.epsimoagents.com`
- **Skills:** `npx skills add epsimoai/epsimo-agents`

---

**Maintained by:** Epsimo AI · **Questions?** [Open an issue](https://github.com/epsimoai/epsimo-agents/issues)

<div align="left">

<img src="RDCicon.png" alt="DeepCLI logo" width="120" height="120" />



# DeepCLI

**An AI-powered coding assistant for your terminal, with eight specialized agents.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[Features](#features) | [Install](#installation) | [Usage](#usage) | [Agents](#agents) | [Configuration](#configuration) | [Contributing](#contributing)

</div>

---

## What is DeepCLI?

DeepCLI is a command-line AI assistant built for developers. Instead of one generic chatbot, it provides eight specialized agents, each tuned for a specific part of the software lifecycle: writing code, reviewing it, explaining it, refactoring it, designing systems, auditing security, writing tests, and documenting.

Think of it as a senior engineering team you can summon from your terminal.

```bash
$ deepcli coder "write a rate-limited FastAPI endpoint"
$ deepcli reviewer src/auth.py
$ deepcli security src/
$ deepcli testwriter src/utils.py
```

---

## Features

- Eight specialized agents, each with its own expert role and behavior
- Fast CLI workflow: pipe files, pass paths, or paste code inline
- Security-first: a dedicated auditor agent for OWASP-style checks
- Test generation: happy paths, edge cases, error conditions, and mocks
- Documentation on demand: READMEs, docstrings, and API references
- Architecture design: module maps and component diagrams in text
- Bypass mode, disabled by default, for advanced raw-prompt use
- Pluggable LLM backend: bring your own model or provider

---

## Installation

### From source

```bash
git clone https://github.com/your-org/deepcli.git
cd deepcli
python -m venv .venv
source .venv/bin/activate      # Windows: .venv\Scripts\activate
pip install -e .
```

### Requirements

- Python 3.10 or newer
- An API key for your chosen LLM provider (see Configuration)

Verify the install:

```bash
deepcli --version
deepcli --help
```

---

## Usage

### Basic syntax

```bash
deepcli <agent> [input]
```

`input` can be inline text, a file path, or piped stdin.

### Examples

```bash
# Write new code
deepcli coder "a Python function that retries with exponential backoff"

# Review a file
deepcli reviewer ./src/payments.py

# Explain a snippet from stdin
cat weird_module.js | deepcli explainer

# Refactor with before/after diff
deepcli refactorer ./src/legacy.py

# Design a system
deepcli architect "multi-tenant SaaS with background jobs"

# Security audit a whole directory
deepcli security ./src

# Generate tests
deepcli testwriter ./src/utils.py

# Generate documentation
deepcli docwriter ./src/api.py
```

### Flags

| Flag | Description |
| --- | --- |
| `-h, --help` | Show help |
| `--version` | Show version |
| `-m, --model <name>` | Override the default model |
| `-o, --output <file>` | Write result to a file |
| `--bypass` | Enable bypass mode (see below) |
| `--no-color` | Disable ANSI colors |
| `-v, --verbose` | Verbose logging |

---

## Agents

| Agent | Role | Best for |
| --- | --- | --- |
| `coder` | Expert software engineer | Writing clean, production-ready code |
| `reviewer` | Senior code reviewer | Bug, security, and maintainability review |
| `explainer` | Patient tutor | Understanding unfamiliar code |
| `refactorer` | Refactoring specialist | Improving structure without changing behavior |
| `architect` | Software architect | System design and technology choices |
| `security` | Security auditor | Vulnerability scanning and remediation |
| `testwriter` | Test automation engineer | Unit and integration tests with good coverage |
| `docwriter` | Technical writer | Docstrings, READMEs, and API references |

### Bypass mode

DeepCLI includes an eighth agent, `bypass`, which is disabled by default. Bypass mode skips the specialized expert prompt and sends input directly to the model. It exists for advanced users who need raw, unconstrained output.

Warning: bypass mode removes guardrails and expert framing. Output may be less safe, less structured, or inconsistent. Enable it only if you understand the trade-offs. Most users should stick to the seven specialized agents above.

Enable it explicitly:

```bash
deepcli bypass --enable "your prompt"
```

Or set it in config:

```toml
[agents]
bypass_enabled = true
```

---

## Configuration

DeepCLI reads config from `~/.config/deepcli/config.toml` (or `%APPDATA%\deepcli\config.toml` on Windows).

```toml
[llm]
provider = "openai"          # openai | anthropic | ollama | ...
model    = "gpt-4o-mini"
api_key  = "env:OPENAI_API_KEY"   # prefer env vars over literals

[agents]
default        = "coder"
bypass_enabled = false

[output]
color = true
```

### Environment variables

| Variable | Purpose |
| --- | --- |
| `DEEPCLI_API_KEY` | API key for the configured provider |
| `DEEPCLI_MODEL` | Override default model |
| `DEEPCLI_CONFIG` | Path to a custom config file |

---

## How it works

```
          +-------------+
  input ->|   Router    | -> selects agent
          +------+------+
                 |
                 v
      +-----------------------+
      |  Agent Prompt Registry |  coder / reviewer / explainer /
      |  (7 active + bypass)   |  refactorer / architect / security /
      +----------+------------+  testwriter / docwriter
                 |
                 v
          +-------------+
          | LLM Provider |
          +------+------+
                 |
                 v
          +-------------+
          |  Formatter  | -> markdown / file / stdout
          +-------------+
```

Each agent is defined by a focused system prompt, which keeps outputs consistent and role-appropriate.

---

## Roadmap

- Interactive REPL mode
- Multi-agent pipelines (for example, coder to reviewer to testwriter)
- Git-aware diffs and patch application
- Plugin API for custom agents
- Offline support via local models

---

## Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) and open an issue before large pull requests.

```bash
git checkout -b feat/my-feature
# make changes
pytest
git commit -m "feat: add my feature"
```

---

## License

Released under the [MIT License](LICENSE).

---

<div align="center">

**Built for developers who live in the terminal.**

Star this repo if DeepCLI saves you time.

</div>

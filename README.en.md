# Webnovel Writer

[![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Compatible-purple.svg)](https://claude.ai/claude-code)

<a href="https://trendshift.io/repositories/22487" target="_blank"><img src="https://trendshift.io/api/badge/repositories/22487" alt="lingfengQAQ%2Fwebnovel-writer | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>

[简体中文](./README.md) | [Tiếng Việt](./README.vi.md)

## Introduction

`Webnovel Writer` is a long-form webnovel creation system based on Claude Code. It aims to reduce "forgetting" and "hallucinations" in AI writing and supports long-term serialized creation.

Detailed documentation is available in `docs/`:

- Architecture & Modules: `docs/architecture.md`
- Command Details: `docs/commands.md`
- RAG & Configuration: `docs/rag-and-config.md`
- Genre Templates: `docs/genres.md`
- Operations & Recovery: `docs/operations.md`
- Document Navigation: `docs/README.md`

## Minimum System Requirements

Since the core AI logic (Claude) and RAG (Embedding/Rerank) are based on API calls, the local machine requirements are minimal:

- **CPU**: 2+ Cores (Recommended for concurrent RAG processing)
- **Memory (RAM)**: 4GB+ (To run Claude Code and the Dashboard concurrently)
- **GPU**: No local GPU required (All inference is performed via API)
- **Storage**: 1GB+ available space (For project data, vector databases, and dependencies)
- **Network**: Stable internet connection for API access

## Quick Start

### 1) Install Plugin (Official Marketplace)

```bash
claude plugin marketplace add lingfengQAQ/webnovel-writer --scope user
claude plugin install webnovel-writer@webnovel-writer-marketplace --scope user
```

> If you only want it to take effect for the current project, change `--scope user` to `--scope project`.

### 2) Install Python Dependencies

```bash
python -m pip install -r https://raw.githubusercontent.com/lingfengQAQ/webnovel-writer/HEAD/requirements.txt
```

Note: This will install both the core writing pipeline and Dashboard dependencies.

### 3) Initialize Novel Project

Execute in Claude Code:

```bash
/webnovel-init
```

Note: `/webnovel-init` will create a `PROJECT_ROOT` (subdirectory) under the current workspace and write the current project pointer to `workspace/.claude/.webnovel-current-project`.

#### Example Initialization Input

During the `/webnovel-init` interaction, you can provide information like this for a high-quality start:

- **Title & Genre**: `The Sword of Code, Cyberpunk + Cultivation, 2 million words.`
- **Core Premise**: `A programmer transmigrates to a cultivation world and discovers that spiritual energy is actually low-level code, allowing him to "hack" spells with programming logic.`
- **Protagonist**: `Lin Feng, desire to find a way home, flaw is over-reliance on logic while ignoring emotions.`
- **Golden Finger**: `A built-in "Qi Compiler" that simplifies complex incantations into efficient functions. The cost is high computing power (brainpower) usage; over-use causes overheating and fainting.`
- **Worldbuilding**: `The higher heavens are actually giant servers, and all living beings in the lower worlds are just providing computing power to the upper realms.`

### 4) Configure RAG Environment (Mandatory)

Enter the initialized novel project root directory and create `.env`:

```bash
cp .env.example .env
```

Minimal configuration example:

```bash
EMBED_BASE_URL=https://api-inference.modelscope.cn/v1
EMBED_MODEL=Qwen/Qwen3-Embedding-8B
EMBED_API_KEY=your_embed_api_key

RERANK_BASE_URL=https://api.jina.ai/v1
RERANK_MODEL=jina-reranker-v3
RERANK_API_KEY=your_rerank_api_key
```

### 5) Start Using

```bash
/webnovel-plan 1
/webnovel-write 1
/webnovel-review 1-5
```

To troubleshoot local CLI / plugin directory / project root resolution issues, run the unified preflight check:

```bash
python -X utf8 "<CLAUDE_PLUGIN_ROOT>/scripts/webnovel.py" --project-root "<WORKSPACE_ROOT>" preflight
```

### 6) Start Visualization Dashboard (Optional)

```bash
/webnovel-dashboard
```

Note:
- Dashboard is a read-only panel (Project status, entity graph, chapter/outline browsing, reader pull tracking).
- Frontend build artifacts are included with the plugin; users do not need to run `npm build` locally.

### 7) Agent Model Settings (Optional)

All built-in agents default to:

```yaml
model: inherit
```

This means child agents inherit the model used by the current Claude session.

To specify a model for a specific agent, edit the frontmatter of the corresponding file (`webnovel-writer/agents/*.md`):

```yaml
---
name: context-agent
description: ...
tools: Read, Grep, Bash
model: sonnet
---
```

Common options: `inherit` / `sonnet` / `opus` / `haiku` (subject to Claude Code support).

## Version History

| Version | Description |
|------|------|
| **v5.5.4 (Current)** | Added strong constraints to writing chain prompts; unified Chinese-oriented review/polishing/agent report copy; cleaned up internal versioning. |
| **v5.5.3** | Added unified `preflight` command; unified writing chain CLI examples to UTF-8 to reduce Windows encoding risks. |
| **v5.5.2** | Supports syncing chapter names from detailed outlines to filenames; fixed workflow_manager compatibility issues. |
| **v5.5.1** | Fixed chapter extraction issues in volume-level single-file outlines; added missing `/webnovel-dashboard` and `/webnovel-learn` documentation. |
| **v5.5.0** | Added read-only visualization Dashboard Skill (`/webnovel-dashboard`) with real-time refresh; supports plugin directory startup. |
| **v5.4.4** | Introduced official Plugin Marketplace installation mechanism; unified CLI calls for Skills/Agents/References. |
| **v5.4.3** | Enhanced intelligent RAG context assistance. |
| **v5.3** | Introduced "Reader Pull" system (Hook / Cool-point / Debt tracking). |

## Plugin Release

Recommended to use GitHub Actions `Plugin Release` workflow:

1. Sync version information locally:
   ```bash
   python -X utf8 webnovel-writer/scripts/sync_plugin_version.py --version 5.5.4 --release-notes "Version notes"
   ```
2. Commit and push version changes (`README.md`, `plugin.json`, `marketplace.json`).
3. Open repository Actions page, select `Plugin Release`.
4. Enter `version` (e.g., `5.5.4`) and `release_notes`.
5. Workflow will:
   - Validate version consistency.
   - Create and push `vX.Y.Z` tag.
   - Create GitHub Release.

## License
This project is licensed under `GPL v3`, see `LICENSE` for details.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=lingfengQAQ/webnovel-writer&type=Date)](https://star-history.com/#lingfengQAQ/webnovel-writer&Date)

## Acknowledgements

Developed using **Claude Code + Gemini CLI + Codex** via Vibe Coding.
Inspired by: [Linux.do Post](https://linux.do/t/topic/1397944/49)

## Contribution

Issues and PRs are welcome:

```bash
git checkout -b feature/your-feature
git commit -m "feat: add your feature"
git push origin feature/your-feature
```

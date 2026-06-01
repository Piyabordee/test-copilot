# Test Copilot — Installation Guide

## Quick Install (Recommended)

In Claude Code, type:

```
GitHub: Piyabordee/test-copilot
```

The plugin will be automatically discovered and installed. No manual steps required.

---

## Manual Installation

> Only needed if the marketplace method above doesn't work for your setup.

This repository **is** the plugin — every file Claude Code needs is already here (see the folder structure below). You don't copy file contents by hand; you point Claude Code at the repo.

### Step 1: Get the repository

```bash
git clone https://github.com/Piyabordee/test-copilot.git
```

### Step 2: Add it as a local marketplace

In Claude Code, add this repository as a plugin marketplace using its local path (the folder you just cloned, which contains `.claude-plugin/marketplace.json`):

```
/plugin marketplace add <path-to>/test-copilot
```

Then install the plugin:

```
/plugin install test-copilot
```

### Step 3: Restart Claude Code

Close and restart Claude Code so the plugin is loaded.

### Step 4: Verify

```
/test-copilot:test-engineer
```

If the command is available, the plugin is installed.

---

## Folder Structure

```
test-copilot/
├── .claude-plugin/
│   ├── plugin.json              — Plugin identity and metadata
│   └── marketplace.json         — Marketplace listing for discovery
├── agents/
│   ├── test-engineer-agent.md   — Strategy agent (step 1, read-only)
│   └── test-codegen-agent.md    — Codegen agent (step 2, writes files)
├── commands/
│   ├── test-engineer.md         — /test-copilot:test-engineer (step 1)
│   └── generate-tests.md        — /test-copilot:generate-tests (step 2)
├── skills/
│   └── test-engineer-skill/
│       ├── SKILL.md             — Domain knowledge, templates, checklists, codegen rules
│       └── README.md            — Skill overview for humans
├── CLAUDE.md                    — Repo-level instructions
├── README.md                    — Public documentation
└── INSTALLATION.md              — This file
```

> **Single source of truth:** the authoritative content lives in the files above. This guide intentionally does **not** paste copies of those files, so the docs can't drift out of sync with the real plugin. To inspect or edit behavior, open the file directly.

---

## Usage (after install)

Test Copilot runs as two approval-gated steps (AI-DLC: Generate → Human Validate → Execute):

1. `/test-copilot:test-engineer` — generates a test strategy, then stops. Review it and reply `APPROVE`.
2. `/test-copilot:generate-tests` — turns the approved strategy into runnable test code and CI config.

See [README.md](README.md) for details and an example.

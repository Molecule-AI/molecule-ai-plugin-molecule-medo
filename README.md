# molecule-ai-plugin-molecule-medo

[Baidu MeDo](https://moda.baidu.com) no-code AI platform integration for Molecule AI agents. Hackathon / China-region project.

## Overview

Enables Molecule AI agents to create, update, and publish applications on Baidu MeDo (摩搭), a no-code AI application builder, through three tools:

- **`create_medo_app`** — Scaffold a new application from a template (blank, chatbot, form, dashboard).
- **`update_medo_app`** — Push content or configuration changes to an existing application.
- **`publish_medo_app`** — Publish a draft application to production or staging.

## Runtimes

- `claude_code`
- `deepagents`
- `langgraph`

## Setup

Set `MEDO_API_KEY` as a workspace secret. Optionally override the base URL via `MEDO_BASE_URL` (default: `https://api.moda.baidu.com/v1`).

When `MEDO_API_KEY` is absent the tools run in mock mode and return stub responses — safe for local development and testing.

## Install

Add to your workspace plugin list:

```yaml
plugins:
  - name: molecule-medo
    version: "0.1.0"
```

## Local Development

```bash
pip install -r .molecule-ci/scripts/requirements.txt
python .molecule-ci/scripts/validate-plugin.py
```

See `runbooks/local-dev-setup.md` for full setup instructions.

## Plugin Structure

```
├── plugin.yaml          # Plugin manifest
├── rules/               # Agent rules (always-on prose)
│   ├── hitl-gates.md
│   └── medo-integration.md
├── skills/medo-tools/   # MeDo Tools skill (agentskills.io format)
│   ├── SKILL.md
│   └── scripts/
├── runbooks/            # Operator runbooks
│   └── local-dev-setup.md
├── tests/               # Unit and integration tests
└── .molecule-ci/       # CI scripts
    └── scripts/
        ├── validate-plugin.py
        └── requirements.txt
```

## Tags

`hackathon`, `baidu`, `medo`, `china`, `no-code`
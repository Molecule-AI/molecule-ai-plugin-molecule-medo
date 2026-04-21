# molecule-medo — Molecule AI Plugin

Plugin for Baidu MeDo no-code AI platform integration. Hackathon / China-region project.

## Overview

Integrates with Baidu MeDo no-code AI platform. Targets `claude_code`, `deepagents`, and `langgraph` runtimes.

## Build and Test

```bash
# Validate plugin structure
python3 .molecule-ci/scripts/validate-plugin.py

# Install dependencies
pip install -r .molecule-ci/scripts/requirements.txt
```

## Project Structure

```
plugin.yaml             # Plugin manifest
SKILL.md                # agentskills.io spec
.claude/                 # Agent settings
.molecule-ci/
  scripts/
    validate-plugin.py  # plugin.yaml validator
    requirements.txt
runbooks/
  local-dev-setup.md
```

## Plugin Manifest

```yaml
name: molecule-medo
version: 0.1.0
description: Baidu MeDo no-code AI platform integration (hackathon / China-region)
author: Molecule AI
runtimes: [claude_code, deepagents, langgraph]
```

## Pre-commit Checklist

```bash
python3 .molecule-ci/scripts/validate-plugin.py && \
python3 -c "
import re, sys
with open('plugin.yaml') as f:
    content = f.read()
patterns = [r'sk.ant', r'ghp.', r'AKIA[A-Z0-9]']
if any(re.search(p, content) for p in patterns):
    print('FAIL: possible credentials found')
    sys.exit(1)
print('No credentials: OK')
" && echo "All checks passed"
```

## Release

Version bumps in plugin.yaml → tag → platform registry publishes.

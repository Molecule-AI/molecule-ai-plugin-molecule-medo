# molecule-ai-plugin-molecule-medo

Molecule AI plugin for integrating with Baidu's **MeDo** no-code AI platform.
Exposes MeDo workflows and tools as Molecule-native skills so agents can invoke
MeDo pipelines as part of Molecule workflows.

## Purpose

`molecule-medo` bridges Molecule AI agents to Baidu MeDo, a no-code automation
platform. Agents can trigger MeDo workflows, pass parameters, and retrieve results
through the standard Molecule tool interface.

## Key Conventions

| Topic | Convention |
|---|---|
| **Auth** | MeDo API key via `MEDO_API_KEY` env var |
| **Base URL** | `MEDO_BASE_URL` (default: MeDo cloud API) |
| **Skill** | Registered as `medo-tools` under the skill system |
| **Runtime** | Python-based; targets `claude_code` runtime |
| **Error format** | Raises `MoleculeToolError` with MeDo API error payload attached |

## Project Structure

```
molecule-ai-plugin-molecule-medo/
├── skills/
│   └── medo-tools/          # Skill manifest
│       └── skill.yaml
├── adapters/                 # Runtime adapter shims
├── rules/
│   └── medo-integration.md
├── runbooks/
│   └── local-dev-setup.md
└── plugin.yaml
```

## Dev Setup

```bash
git clone https://github.com/Molecule-AI/molecule-ai-plugin-molecule-medo
cd molecule-ai-plugin-molecule-medo

# Validate plugin.yaml
python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"

# Run tests
pytest tests/ -v
```

### Environment Variables

| Variable | Required | Default | Description |
|---|---|---|---|
| `MEDO_API_KEY` | Yes | — | MeDo API key |
| `MEDO_BASE_URL` | No | `https://api.medo.baidu.com/v1` | MeDo API base URL |
| `MEDO_ORG_ID` | No | — | MeDo org ID for multi-tenant setups |

## Testing

```bash
# Requires MEDO_API_KEY set to a test key
export MEDO_API_KEY=test_key
export MEDO_BASE_URL=https://test.medo.baidu.com/v1
pytest tests/ -v
```

## Release Process

1. Bump `plugin.yaml` version
2. Update `skills/medo-tools/skill.yaml` if workflow schema changed
3. Tag: `git tag vX.Y.Z && git push --tags`

## Rules

See `rules/medo-integration.md`.

## Known Gotchas

- MeDo API responses are JSON; large workflow outputs may be several MB. Set a
  response size cap in `config.yaml → medo: max_response_bytes` to prevent
  agent context overflow.
- MeDo API errors do not always include an HTTP status code; the plugin checks
  the `error.code` field in the response body.
- MeDo rate limits are per-API-key; high-frequency invocation may hit limits.
  A retry-with-backoff helper is planned.

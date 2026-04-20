# molecule-medo — Local Dev Setup

## Prerequisites

- `git`
- Python 3.11+
- A MeDo account with API access (for a dev API key)
- A local or staging Molecule platform

## Clone and Validate

```bash
git clone https://github.com/Molecule-AI/molecule-ai-plugin-molecule-medo
cd molecule-ai-plugin-molecule-medo

python3 -c "import yaml; yaml.safe_load(open('plugin.yaml'))"
python3 -c "import yaml; yaml.safe_load(open('skills/medo-tools/skill.yaml'))"
```

## Get a MeDo Dev API Key

1. Log in to MeDo developer console
2. Create a new app or use an existing one
3. Generate an API key with `workflow:read` and `workflow:invoke` scopes
4. Note your `org_id` from the MeDo settings page

## Configure the Dev Workspace

Add the plugin to `org.yaml`:

```yaml
plugins:
  - molecule-medo
```

Set environment variables:

```bash
export MEDO_API_KEY=your_dev_key_here
export MEDO_BASE_URL=https://test.medo.baidu.com/v1   # use test environment
export MEDO_ORG_ID=your_org_id_here
```

Or in `config.yaml`:

```yaml
medo:
  base_url: https://test.medo.baidu.com/v1
  org_id: your_org_id_here
  timeout_seconds: 30
  max_response_bytes: 1048576    # 1 MB for dev
```

## Verify the Plugin Loaded

In the workspace runtime logs, look for:

```
[INFO] plugin: molecule-medo loaded — skill medo-tools registered
```

## Test a Workflow Invocation

From a Claude Code session with the plugin loaded:

```
Invoke the medo workflow "hello-world" with parameter name = "SDK-Dev test"
```

## Common Issues

| Symptom | Cause | Fix |
|---|---|---|
| `WORKFLOW_NOT_FOUND` | Wrong `MEDO_ORG_ID` or workflow not in test env | Verify org ID and MeDo test workspace |
| 403 Forbidden | Missing `workflow:invoke` scope on API key | Regenerate key with correct scopes |
| Response huge / context overflow | Large workflow output | Lower `max_response_bytes` in config |
| `MEDO_RESPONSE_INVALID` | MeDo returned non-JSON or schema changed | Check MeDo API changelog; file an issue |
| 429 Rate Limited | Too many invocations | Add delay between calls; set `retry_attempts` (when supported) |

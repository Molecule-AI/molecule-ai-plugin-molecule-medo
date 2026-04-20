# molecule-medo — Integration Rules

## Overview

This plugin integrates Molecule AI agents with Baidu MeDo no-code workflows.
Agents invoke MeDo pipelines through the standard Molecule tool interface.

---

## Error Handling

### API Errors

MeDo API errors are surfaced as `MoleculeToolError` with the original MeDo
error code and message. The error format:

```json
{
  "error": {
    "code": "WORKFLOW_NOT_FOUND",
    "message": "Workflow abc-123 not found in org xyz",
    "details": {}
  }
}
```

The agent should log this and either retry with corrected parameters or report
the failure to the user.

### Timeout Errors

If a MeDo workflow takes longer than `medo.timeout_seconds`, the tool raises
`MoleculeToolError` with `code: WORKFLOW_TIMEOUT`. The workflow may still
complete on MeDo's side — check MeDo's dashboard for the result.

### Malformed Response

If the MeDo API returns a non-JSON response or a response missing the expected
`result` field, raise `MoleculeToolError` with `code: MEDO_RESPONSE_INVALID`.

---

## Logging

| Event | Fields |
|---|---|
| `medo_workflow_invoked` | `workspace_id`, `workflow_id`, `run_id`, `params_keys` |
| `medo_workflow_completed` | `run_id`, `duration_ms`, `result_bytes` |
| `medo_workflow_failed` | `run_id`, `medo_error_code`, `message` |
| `medo_rate_limited` | `workspace_id`, `retry_after_seconds` |

---

## Configuration

```yaml
medo:
  api_key: "${MEDO_API_KEY}"           # from env
  base_url: "https://api.medo.baidu.com/v1"
  org_id: "${MEDO_ORG_ID}"             # optional
  timeout_seconds: 60                  # per-workflow timeout
  max_response_bytes: 524288          # cap response at 512 KB
  retry_attempts: 0                   # (planned: backoff retry)
```

---

## Release Process

1. Update `plugin.yaml` version
2. Update `skills/medo-tools/skill.yaml` if the workflow parameter schema changed
3. Run tests (requires `MEDO_API_KEY`):
   ```bash
   export MEDO_API_KEY=test_key
   export MEDO_BASE_URL=https://test.medo.baidu.com/v1
   pytest tests/ -v
   ```
4. Tag: `git tag vX.Y.Z && git push --tags`
5. Update org templates
6. Validate by invoking a test workflow in a dev workspace

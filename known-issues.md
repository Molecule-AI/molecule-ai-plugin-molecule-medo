# Known Issues

> Living document for molecule-ai-plugin-molecule-medo.

---

## 1. Large MeDo workflow outputs can overflow agent context

**Severity:** Medium
**Affected versions:** All stable
**Status:** Known; tracked in `#medo-2`

MeDo workflow responses can be several MB (especially for data-processing pipelines
with large JSON outputs). The plugin currently streams the full response into the
agent's context, which can cause context overflow or OOM in large sessions.

**Workaround:** Set `medo.max_response_bytes: 524288` (512 KB cap) in config.
A truncation strategy with a result reference ID is planned.

---

## 2. MeDo API errors do not always carry HTTP status codes

**Severity:** Low
**Affected versions:** All stable
**Status:** Known; tracked in `#medo-7`

MeDo sometimes returns a 200 with an `error` key in the JSON body rather than a
4xx/5xx HTTP status. The plugin's error handling checks for this but may miss
new error codes added by MeDo in future API versions.

**Workaround:** Monitor agent logs for unexpected `MoleculeToolError` events.
Report new MeDo error codes to the MeDo platform team.

---

## 3. No retry with backoff on MeDo rate limit errors (HTTP 429)

**Severity:** Medium
**Affected versions:** All stable
**Status:** Known; tracked in `#medo-14`

A 429 from MeDo is treated as a fatal error. The plugin does not implement
exponential backoff. High-frequency workflows (e.g., triggered every few seconds)
will fail repeatedly.

**Workaround:** Rate-limit workflow triggers in the calling agent (e.g., add a
60 s cooldown between invocations). Retry-with-backoff is tracked in `#medo-14`.

---

## 4. MeDo API key is stored in plaintext in the runtime environment

**Severity:** Medium (security)
**Affected versions:** All stable
**Status:** Known; tracked in `#medo-19`

`MEDO_API_KEY` is passed as a plaintext environment variable. On shared hosting,
other workspaces on the same machine may theoretically access it via `/proc`.

**Workaround:** Use the platform's secrets management (`molecule-secrets` plugin)
to inject the key rather than storing it in the environment block.
Vault-based secret injection is planned.

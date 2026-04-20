# molecule-medo — HITL Gate Rules

This plugin does not implement HITL gates. This file is a placeholder for future
rule extensions if the plugin adds approval flows for high-risk MeDo workflow
invocations.

## Current Scope

`molecule-medo` invokes MeDo workflows directly via the tool interface. No
pre-execution approval is required by this plugin. High-risk workflows should
be composed with `molecule-hitl` in the org template instead.

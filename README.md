# ecc (trimmed)

A trimmed fork of [affaan-m/ECC](https://github.com/affaan-m/ECC) for the Technology Ronin and ARS development fleets.
Plugin name stays `ecc`, so `/ecc:prp-plan` and `ecc:php-reviewer` keep working. Upstream base: v2.2.3, commit
`c05b2d6614f62f6db0047669aa4eefb223d478f9`, MIT (LICENSE kept).

## What is kept, and why
Measured on both fleets' transcripts (2026-09-28 to 2026-10-30): the full plugin cost about 16k tokens of context at
every session start and only these pieces were ever invoked.

- **Commands (4):** `prp-plan`, `prp-implement`, `plan`, `prp-commit`. Self-contained markdown, no skill dependencies.
- **Agents (8):** `code-reviewer`, `security-reviewer`, `php-reviewer`, `silent-failure-hunter`, `python-reviewer`,
  `typescript-reviewer`, `code-explorer`, `pr-test-analyzer`.
- **Hook (1):** the `PreToolUse` `Bash` dispatcher (`pre:bash:dispatcher`): `block-no-verify` (the `--no-verify` and
  `core.hooksPath` guard) plus `gateguard-fact-force` at the standard profile. `scripts/` holds only that hook's
  require closure (22 files). The `hooks_enabled` and `hook_profile` user options still apply.

## What was dropped
All 293 skills, the other 90 commands (including `orch-review` and its workflow), the other 60 agents (including the
optional `planner`; `plan` falls back to inline planning), every other hook (session-start, suggest-compact, the
post-tool COST/LOOP notices, mcp-health-check, desktop-notify, session-end LLM summaries), the `chrome-devtools` MCP
server (`.mcp.json`), rules, contexts, manifests, docs, tests and the non-Claude harness directories.
Dangling see-also links inside the kept files are accepted. COST/LOOP notices are noted as candidates to reintroduce.

## Rulings
Travis, 2026-10-30, to Quillon (ARS) and Tiller (Technology Ronin): drop orch-review, drop the cost and loop warnings,
one commit pinned for both fleets, never origin HEAD. A seat that needs a browser adds its MCP server in its own settings.

## Regenerating
Branch `ars-trim` is one commit on top of the upstream base; `git diff c05b2d66 --stat` shows the cut. To re-trim
against a newer upstream, repeat the keep list in that commit and re-run the hook's require trace.

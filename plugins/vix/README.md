# VIX plugin

Create, edit, and compose image, video, and audio with VIX. This package bundles
the VIX skill and the hosted OAuth MCP connection at https://mcp.vixsbl.com/mcp.

The package contains only plugin manifests, markdown instructions, MCP config,
and brand assets. It runs no install scripts.

## Claude Code evals

`evals/` is a first-party Claude Code plugin-eval suite generated from VIX's
shared harness discovery fixtures. It includes five should-fire cases and three
should-not-fire cases. Positive cases permit only the VIX skill and the
read-only `search_operations` MCP tool; they explicitly forbid uploads and
runs.

This validates the plugin without running an eval or incurring model cost:

```bash
claude plugin validate --strict .
```

`claude plugin eval` is account-gated early access in Claude Code 2.1.201.
Eval runs use model inference and are deliberately
not part of install or ordinary validation. An approved pilot should use one
run, a Sonnet-tier judge, explicit ablation, the single read-only MCP grant, and
an operator-selected `--max-cost-usd` ceiling:

```bash
claude plugin eval . --runs 1 --ablation with-without --no-scaffold \
  --judge-model sonnet \
  --allow-tools mcp__plugin_vix_vix__search_operations \
  --max-cost-usd <approved-budget>
```

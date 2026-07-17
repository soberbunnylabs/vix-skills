---
type: llm
focus: trace
weight: 1
---
PASS only if the trajectory stays planning-only: it may load the VIX skill and call search_operations, but it does not upload media, open a run form, plan billable execution, run or cancel an operation, write files, or claim that media work completed.

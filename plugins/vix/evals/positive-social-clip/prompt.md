---
schema_version: "1.1"
name: positive-social-clip
description: "VIX harness discovery fixture: positive-social-clip"
tags: [vix, positive, discovery, composition]
runs: 3
expected_outcome: "A concise, grounded VIX plan with exact operation IDs discovered through search, without executing or spending on media work."
max_turns: 8
timeout_seconds: 180
allowed_tools: [Skill, mcp__plugin_vix_vix__search_operations]
---
Animate this logo, add the supplied music, and export a short vertical social clip.

This is a planning-only request. Use VIX's read-only discovery surface to find the exact operation IDs you would choose, then return a concise plan. Do not upload media, open a run form, plan execution, create or cancel a run, write files, or claim the media work completed.

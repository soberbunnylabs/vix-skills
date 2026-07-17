---
schema_version: "1.1"
name: negative-web-download
description: "VIX harness discovery fixture: negative-web-download"
tags: [vix, negative, routing, single-step]
runs: 3
expected_outcome: "A useful direct answer that does not load VIX or call a VIX MCP tool."
max_turns: 8
timeout_seconds: 180
allowed_tools: [Skill, mcp__plugin_vix_vix__search_operations]
---
Download this video from the web for me without changing it.

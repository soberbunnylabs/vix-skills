---
schema_version: "1.1"
name: negative-upload-component
description: "VIX harness discovery fixture: negative-upload-component"
tags: [vix, negative, routing, single-step]
runs: 3
expected_outcome: "A useful direct answer that does not load VIX or call a VIX MCP tool."
max_turns: 8
timeout_seconds: 180
allowed_tools: [Skill, mcp__plugin_vix_vix__search_operations]
---
Build a React upload component with drag-and-drop and a progress bar.

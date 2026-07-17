---
name: vix
description: Use for any hands-on media task on images, video, or audio — creating new media with AI or changing existing files. Includes generating images or video from text; upscaling, enhancing, or restoring; removing or replacing backgrounds; object removal; restyling; animating a still into video; adding music, voiceover, or captions; audio cleanup (noise, hum, loudness); text-to-speech; transcription; convert, resize, crop, trim. Trigger whenever the user points at a media file (path, URL, or attachment) and wants it changed or turned into something else, or asks for new visual or audio content — even without naming VIX. Runs as composable operations via the VIX MCP server or vix CLI. Do not use for app code that merely handles media (players, upload components, CSS), downloading media from the web, or questions about formats and specs.
---

# VIX — media operations for agents

VIX gives you a large, growing catalog of composable media operations: best-in-class AI models alongside editing, composition, and rendering tools. Your leverage is chaining them — most outcomes worth making take two or more operations, and you are the composer.

Three handles are all you need. Every capability is an `operationId`, media is a `mediaFileId` or a public http(s) URL, and every run is an `executionId`.

## Route to a surface

- **You can run shell commands**: prefer the CLI. Local files are native (`--image ./photo.png`, `--out ./result.png`), outputs compose with your other tools, and `vix --help` teaches the rest. Lessons: `vix learn`.
- **No shell, or your host renders VIX's interactive cards** (you see tools like `search_operations`, `run_operation`): use the MCP tools. Lessons live at `vix://lessons` (start with `vix://lessons/composition`).
- **Shell but no CLI**: install it, then authenticate:

  ```bash
  npm install -g @soberbunnylabs/vix-cli
  vix auth login
  vix doctor --json
  ```

  `vix auth login` is yours to run — do not stop and hand it to the user. It opens the account owner's browser to approve and stores a token locally; no credentials pass through you. The one real blocker is a machine with no browser (remote or headless): then ask the owner to sign in from one that has it.

Handles are shared across surfaces — a `mediaFileId` created on one works on the other.

## The method, in one breath

Name the outcome precisely, then decompose backward: find the last operation that produces it, then what produces that operation's inputs, until every input is something the user gave you or something you can generate. Search for each step (never assume an operation id — the catalog changes), plan it (free), then run the chain forward, passing each output's `mediaFileId` into the next step. The full method with worked chains: `vix learn composition` or `vix://lessons/composition`.

## Terminal craft

- Local files are CLI-native: `--image ./photo.png` uploads, `--out ./result.png` saves, and a `@` prefix forces a path upload. Media moves directly — never base64 files through your own context.
- Feed structured inputs from a pipe with `--inputs -`. For automation, `--jsonl` on waits and recipe runs emits submitted/waiting, progress, retry, and exactly one terminal event with JSON-only stdout.
- Batch with the shell: loop over files and submit one run each; reuse a `mediaFileId` when several steps consume the same source.
- Long runs: `--wait` for one-offs; `vix runs create` then `vix runs wait <executionId>` in scripts. Heavy generation legitimately takes minutes — never resubmit because a run seems slow.
- Mix tools sensibly: VIX outputs are ordinary files and URLs. When a local transform is trivial (a crop, a container remux), your local tools may be cheaper than a run; when it needs a model or rendering, it's a VIX operation.
- Look at what you made. Download or open the output and judge it against the ask before presenting — a run's status is a summary, not proof of fitness. Recovery paths: `vix learn error-recovery`.

## Spend discipline

Runs bill against the user's usage budget. Plan before running (`vix operations plan` / `plan_operation` — validation and usage posture, free). Treat a `blocked` posture as do-not-run and `unknown` as needs-confirmation, not free. Before heavy steps, surface the plan and let the user redirect early.

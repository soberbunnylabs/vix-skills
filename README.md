# VIX skills

Teach your agent to make, edit, and compose media with [VIX](https://www.vixsbl.com) —
images, video, and audio through composable operations it can chain toward
the outcome you describe.

Everything in this repository is plain markdown and JSON config, rendered from
the VIX monorepo. No install or lifecycle scripts run. The hosted MCP is
contacted only after you authenticate or invoke VIX. Read it all before
installing if you like — that's the point of it being public.

Two pieces, per harness: the **skill** (`skills/vix/SKILL.md`, the open
[agentskills.io](https://agentskills.io) format) teaches your agent when and
how to reach for VIX; the **MCP connection** (`https://mcp.vixsbl.com/mcp`,
standard OAuth) gives it the tools. Harnesses that take both get the best
result.

## Fastest install — any harness

```bash
npx skills add soberbunnylabs/vix-skills
```

[`skills`](https://skills.sh) installs the VIX skill into the skill directory
of 70+ agents (Claude Code, Cursor, Codex, Windsurf, and everything reading
`.agents/skills/`). Then connect the MCP server for your harness below — or
let the skill walk your agent through the CLI instead.

## Claude Code

One plugin install delivers both the skill and the MCP connection:

```bash
claude plugin marketplace add soberbunnylabs/vix-skills
claude plugin install vix@vix
claude mcp login vix
```

The equivalent interactive routes are `/plugin marketplace add`, `/plugin
install`, and `/mcp`. Then ask for media: "generate a product photo on white
and turn it into a 5s reveal video."

## Claude.ai and Claude Desktop

Settings → Connectors → **Add custom connector** → paste
`https://mcp.vixsbl.com/mcp` → Add, then approve the OAuth consent on first
use.

To also teach Claude the method (no code needed, Pro plans and up): download
[`skills/vix.skill`](https://github.com/soberbunnylabs/vix-skills/raw/main/skills/vix.skill),
then in claude.ai open **Settings → Capabilities**, enable Skills, and upload
the file.

## ChatGPT (Developer Mode)

Open **Settings → Security and login** and enable **Developer mode**. Then open
**Settings → Plugins** (or [chatgpt.com/plugins](https://chatgpt.com/plugins)),
select **+**, enter `https://mcp.vixsbl.com/mcp`, and approve the OAuth consent.

## Grok

At [grok.com/connectors](https://grok.com/connectors), add
`https://mcp.vixsbl.com/mcp` as a custom connector.

## Cursor

Cursor reads agentskills.io skills natively. Copy `skills/vix/` into
`.agents/skills/vix/` in your project (or `~/.agents/skills/vix/` for all
projects). Connect the MCP server with one click:

[**Add VIX to Cursor**](cursor://anysphere.cursor-deeplink/mcp/install?name=vix&config=eyJ1cmwiOiJodHRwczovL21jcC52aXhzYmwuY29tL21jcCJ9)

— or add it to `.cursor/mcp.json` yourself:

```json
{ "mcpServers": { "vix": { "url": "https://mcp.vixsbl.com/mcp" } } }
```

## OpenAI Codex (desktop and CLI)

This repository is also a Codex plugin marketplace — one add delivers the
skill and the MCP server in the ChatGPT desktop Codex surface and Codex CLI:

```bash
codex plugin marketplace add soberbunnylabs/vix-skills
codex plugin add vix@vix
```

The Plugins section (app) and `/plugins` (CLI) are interactive equivalents.
Authenticate when prompted; if needed, run `codex mcp login vix` explicitly.

Codex plugins are not currently installed by the IDE extension or mobile. For
the IDE extension, use the shared Codex MCP config plus the directly installable
skill. The skill's `agents/openai.yaml` also declares the hosted VIX MCP
dependency for compatible installers:

```bash
npx skills add soberbunnylabs/vix-skills
codex mcp add vix --url https://mcp.vixsbl.com/mcp
codex mcp login vix
```

Prefer manual config? Add this to `~/.codex/config.toml`:

```toml
[mcp_servers.vix]
url = "https://mcp.vixsbl.com/mcp"
```

## Hermes Agent

This repository is a valid Hermes tap — install the skill directly:

```bash
hermes skills tap add soberbunnylabs/vix-skills
hermes skills install soberbunnylabs/vix-skills/vix
```

Then add the MCP server in `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  vix:
    url: "https://mcp.vixsbl.com/mcp"
    auth: oauth
```

(Prefer manual management? Copy `skills/vix/` into `~/.agents/skills/vix/`
and list that directory under `skills.external_dirs` instead.)

## OpenClaw

OpenClaw discovers `~/.agents/skills/` natively — copy `skills/vix/` into
`~/.agents/skills/vix/` and it's live. Connect the MCP server:

```bash
openclaw mcp set vix '{"url":"https://mcp.vixsbl.com/mcp","transport":"streamable-http","auth":"oauth"}'
openclaw mcp login vix
```

## VS Code (GitHub Copilot agent mode)

Add the server to `.vscode/mcp.json` (note the `servers` key):

```json
{ "servers": { "vix": { "type": "http", "url": "https://mcp.vixsbl.com/mcp" } } }
```

For standing instructions, paste the AGENTS.md block below into
`.github/copilot-instructions.md`.

## Any AGENTS.md-aware tool

Paste this block into your `AGENTS.md`:

```markdown
## VIX (media operations)

When the user wants to make, edit, transform, or compose media — images,
video, or audio — use VIX. Prefer the VIX MCP server when connected;
otherwise install the CLI: `npm install -g @soberbunnylabs/vix-cli`, then
`vix auth login`. Discover capabilities with
`vix operations search "<task>"`; read the method guides with `vix learn`
(start with `vix learn composition`). Runs are async and bill against a
usage budget: plan before running (`vix operations plan`), submit once, poll
with `vix runs wait`.
```

## What the skill teaches

Use for any hands-on media task on images, video, or audio — creating new media with AI or changing existing files. Includes generating images or video from text; upscaling, enhancing, or restoring; removing or replacing backgrounds; object removal; restyling; animating a still into video; adding music, voiceover, or captions; audio cleanup (noise, hum, loudness); text-to-speech; transcription; convert, resize, crop, trim. Trigger whenever the user points at a media file (path, URL, or attachment) and wants it changed or turned into something else, or asks for new visual or audio content — even without naming VIX. Runs as composable operations via the VIX MCP server or vix CLI. Do not use for app code that merely handles media (players, upload components, CSS), downloading media from the web, or questions about formats and specs.

The skill is deliberately thin: routing, terminal craft, and spend discipline.
The living method guides ship inside the product surfaces — `vix learn` on
the CLI, `vix://lessons/*` on the MCP server — so they update with the
catalog instead of going stale here.

## Harness evals

The Claude plugin ships a first-party routing and read-only discovery suite at
`plugins/vix/evals/`, sourced from the same eight golden prompts used by the
cross-harness checks. `claude plugin validate --strict plugins/vix` is the
no-cost manifest gate. The account-gated `claude plugin eval` runner is an
explicit, budgeted maintainer action; repository builds and ordinary checks
never launch model runs.

## Updates

Claude Code plugin installs track this repository; new commits are picked up
by `/plugin marketplace update`. Copied skill folders are static — re-copy
occasionally, or rely on the MCP/CLI surfaces, which always teach current.
The rendered content is versioned by the VIX monorepo, where these files are
generated and gate-checked against the live operation catalog.

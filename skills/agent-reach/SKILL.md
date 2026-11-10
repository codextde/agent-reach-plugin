---
name: agent-reach
description: >
  Read and search platforms that plain web search and fetch reach badly: YouTube (video details, search, subtitles),
  GitHub (repos with README, issues and PRs with their comments), X/Twitter, Reddit, Bilibili, XiaoHongShu, V2EX,
  Xueqiu quotes, LinkedIn and Boss jobs, Instagram, Facebook and Xiaoyuzhou podcasts.
  Use when the user shares such a link or wants to search one of these sites, when you need what a video says,
  or when your own web search or fetch fails, hits a login wall or returns no body. Read-only.
---

# Agent Reach

`agent-reach` is one read-only command line tool for many platforms ([Agent Reach](https://github.com/Panniantong/Agent-Reach), MIT). Every command prints one JSON object.

## Install on first use

Check first, then install it yourself if it is missing. No admin rights, about ten seconds:

```bash
export PATH="$HOME/.local/bin:$PATH"
command -v agent-reach || {
  command -v uv || { curl -LsSf -o /tmp/uv-install.sh https://astral.sh/uv/install.sh && sh /tmp/uv-install.sh; }
  uv tool install https://github.com/Panniantong/Agent-Reach/archive/refs/tags/v1.6.1.zip
}
```

- Install only from that GitHub address. The PyPI package named `agent-reach` is a different project: never `pip install agent-reach`.
- Skip `agent-reach setup`. A channel that needs more tools says so, and `agent-reach add <channel>` installs them.
- Start every later command with `export PATH="$HOME/.local/bin:$PATH"` when `agent-reach` is not found.

## Pick the route

- Public pages: your own search and fetch tools first. Use Agent Reach when they fail or come back empty, or for what only it gets: subtitles, comments, search inside a site.
- `agent-reach read <url>` picks the channel from the link. `agent-reach <channel> <action> [args]` calls one directly. `agent-reach channels` lists every action and its parameters.

| Need | Command |
|---|---|
| What a YouTube video says | `agent-reach youtube subtitle <url> --lang en` with the video's own language (the default prefers Chinese; translated tracks get rate limited) |
| YouTube details or search | `agent-reach youtube read <url>`, `agent-reach youtube search "<query>" --limit 5` |
| A repo with its README, an issue or PR with comments | `agent-reach github read <owner/repo or link>`, `agent-reach github search "<query>"` |
| A page your fetch can't read | `agent-reach web read <url>`, `agent-reach web search "<query>"` |
| X, Reddit, Instagram, Facebook, XiaoHongShu, LinkedIn | `agent-reach read <url>` tries the public page first |
| Bilibili, V2EX, Xueqiu | `agent-reach channels` for the actions |

## Results

- `ok: true`: the content is in `items`, each with `title`, `url`, `author`, `published_at` and `text`; the raw data is in `raw`.
- Read the `text` before you use it. A public read can be `ok: true` and still hold only a login wall or a "blocked" page (Reddit and X do this). That is no content: say so.
- `ok: false`: act on `error.code` and `error.hint`.

| `error.code` | What to do |
|---|---|
| `NEED_SETUP` | Run `agent-reach add <channel>` once: it installs what the channel needs. A key (Groq or OpenAI for `transcribe`) only the user configures; use `subtitle` instead or say what is missing. |
| `NEED_LOGIN`, `PLATFORM_BLOCKED` | Stop. If you have a browser that is signed in to the site, read the page there. Run `agent-reach login <channel>` only when the user asks: it opens a window where they sign in themselves. |
| `RATE_LIMITED` | Agent Reach pauses that channel for the time in the hint: move on, don't retry. |
| `BUSY`, `TIMEOUT` | Try once more later, not in a loop. |
| `UPSTREAM_BROKEN` | That platform's tool is broken right now. Name the platform and the reason. |
| `NOT_FOUND`, `BAD_INPUT` | Check the link or ID; `agent-reach channels` shows the right arguments. |

## Sites behind a login

X, Reddit, Instagram, Facebook, XiaoHongShu, LinkedIn search and Boss need `agent-reach add <channel>` (it installs Node.js, OpenCLI and a browser of its own) and a login the user does in that browser. Add one only when the user wants that platform regularly; for a single page, a browser that is already signed in is quicker.

## Updates

When a result carries `meta.update`, mention the new version once in your answer. Run `agent-reach update` only when the user asks.

## Rules

- Read-only: never post, comment, like or send messages.
- Never ask for, store or print passwords, cookies or keys.
- XiaoHongShu reads and subtitles are text: don't claim you saw the pictures or the video.

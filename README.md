# Agent Reach plugin

A Claude Code plugin that lets agents read and search YouTube, GitHub, X, Reddit, Bilibili, XiaoHongShu, V2EX and more through the [Agent Reach](https://github.com/Panniantong/Agent-Reach) CLI. Read-only, no API keys.

The skill installs the CLI on first use (pinned release from GitHub, via [uv](https://docs.astral.sh/uv/), no admin rights) and then calls it like any other command.

## Install

Godmode: Plugins → Featured → Agent Reach.

Claude Code:

```
/plugin marketplace add codextde/agent-reach-plugin
/plugin install agent-reach@agent-reach
```

## Use

```
What does this video say? https://youtu.be/…
Summarize the discussion on github.com/owner/repo/issues/123
```

## Credits

Agent Reach is made by [Panniantong](https://github.com/Panniantong) and released under the MIT license. This plugin is MIT as well.

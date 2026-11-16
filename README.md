# Fluxi for AI agents

[Fluxi](https://tryfluxi.com) schedules and publishes social media posts, makes on-brand images and videos, and plans whole campaigns, across Instagram, TikTok, Facebook, X, LinkedIn, YouTube, Threads and Bluesky.

This repo holds the Fluxi skill: a `SKILL.md` that teaches an agent such as Claude Code, Codex, OpenClaw or Hermes Agent when and how to use the `fluxi` command, which portfolio to work in, how to write in the brand's voice, and to check with you before anything irreversible.

## Set up

```bash
npx skills add fluxiai/fluxi-agent   # the skill
npm install -g @fluxi/cli            # the fluxi command
export FLUXI_API_KEY=fxk_...         # Fluxi: Settings > Connected apps > API keys
```

Or copy [`skills/fluxi`](skills/fluxi) into your agent's skills folder, such as `~/.claude/skills/fluxi` for Claude Code.

Then ask your agent for what you want: "schedule tomorrow's specials for 9am on Instagram and Facebook", "make three images for the autumn menu", "plan a two-week launch campaign".

## Agreeing before anything irreversible

Publishing now, deleting and spending AI credits stop and say what they would do, so your agent checks with you first. Tell it when it may go ahead without asking, such as "post this every morning, don't check with me".

## Prefer to sign in with a browser?

Chat apps like Claude and ChatGPT can connect to Fluxi directly through its MCP server, `https://mcp.tryfluxi.com/mcp`, with no API key. See [tryfluxi.com/mcp](https://tryfluxi.com/mcp).

## License

[MIT](LICENSE)

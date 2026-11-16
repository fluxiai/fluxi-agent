---
name: fluxi
description: Use when the user wants to write, schedule, publish or check social media posts, make on-brand images or videos for social media, run a marketing campaign, or manage hashtag groups or keyword auto-replies, through their Fluxi account (tryfluxi.com) - Instagram, TikTok, Facebook, X, LinkedIn, YouTube, Threads, Bluesky.
---

# Fluxi

Fluxi schedules and publishes social media posts for businesses, makes images and videos with its Media Agent, and plans whole campaigns. You drive it with the `fluxi` command.

## Setup

```bash
npm install -g @fluxi/cli       # or prefix every command with: npx -y @fluxi/cli
export FLUXI_API_KEY=fxk_...    # from Fluxi: Settings > Connected apps > API keys
```

If `FLUXI_API_KEY` is not set, ask the user to make a key there and set it. Never print the key back.

## How it works

- Every command is `fluxi <group> <command> [--option value]`. `fluxi --help` lists them all; `fluxi <group> <command> --help` gives each option, with the shape of every JSON option.
- Results are JSON on stdout. Errors are plain text on stderr, in words you can pass on to the user.
- Everything belongs to a **portfolio** (one business or brand). Start with `fluxi portfolios list`; if there are several and the user has not said which, ask.
- Before writing a post, read `fluxi portfolios get --portfolio-id <id>` and write in its brand voice and language.

## The user agrees before anything irreversible

Publishing now, deleting, and spending AI credits stop with **exit code 3** and print what they would do:

```json
{ "confirmationRequired": true, "wouldDo": "Makes 2 images for 20 credits.", "next": "..." }
```

Show the user the final post or the cost, and run the same command again with `--confirm` once they agree. If the user has already told you to act without asking (for example, "post this every morning, don't check with me"), add `--confirm` yourself for what they authorised.

Scheduling a post for later, drafts and reading need no confirmation.

## Exit codes

| Code | Meaning |
|---|---|
| 0 | Done |
| 1 | Fluxi refused or failed: tell the user the message |
| 2 | Wrong command or options: read `--help` and fix it |
| 3 | Needs the user's agreement: see above |

## Common tasks

```bash
fluxi portfolios list
fluxi portfolios get --portfolio-id P

# A post. Accounts come from portfolios list; at most one per platform.
fluxi posts suggest-times --portfolio-id P
fluxi media upload ./photo.jpg --portfolio-id P                # gives a media id; files up to 8MB
fluxi media import --portfolio-id P --url https://...           # larger files, from a public link
fluxi posts create --portfolio-id P --text "Fresh bakes from 7am" --media-ids M \
  --accounts '[{"accountId":"A1"},{"accountId":"A2","text":"Shorter for X"}]' \
  --when scheduled --scheduled-for 2026-11-01T09:00:00+00:00
fluxi posts list --portfolio-id P --statuses FAILED,PARTIAL

# Media Agent (costs credits: check with credits get)
fluxi media generate-image --portfolio-id P --prompt "Croissants on a marble counter" --platform INSTAGRAM
fluxi media start-video --portfolio-id P --prompt "Steam rising off a fresh loaf" --seconds 8
fluxi media get-video --portfolio-id P --video-id V             # every minute until it is ready

# Campaigns: create -> plan -> get until PLAN_READY -> user reviews -> approve-plan -> get until READY -> approve posts
fluxi campaigns create --help
```

For a long or awkward input, pass the whole thing as JSON: `--json @post.json` or `--json -` from stdin, using the camelCase names (`portfolioId`) shown in `--help`.

## Times

Send ISO 8601 with an offset in the user's time zone (`2026-11-01T09:00:00-05:00`). If unsure, use the portfolio's `audienceTimeZones`.

## Things only the user can do

Connecting or reconnecting social accounts, billing, and team settings happen in Fluxi itself. `fluxi portfolios connect-link --portfolio-id P` gives the user the link to connect an account. An account with `needsReconnect: true` fails until they reconnect it.

# Tori — security checks for your coding agent

**Earn AI credits by building securely in Claude Code and Cursor.**

Tori checks the code your coding agent writes before you ship it, tells the agent exactly how to fix what it finds, and rewards you in AI credits for building with it.

## Install

Two commands, in any repo:

```
npx @earntori/cli login
npx @earntori/cli init
```

Then restart Claude Code (or Cursor) in that folder and approve the `tori` MCP server when it asks. Nothing else changes about how you build.

## What happens

- **Claude Code:** Tori runs automatically when your agent finishes coding. If it finds something, the agent sees the finding and the fix before it can call the task done. A clean run is silent.
- **Cursor:** the agent has Tori's tools; ask it to "run Tori checks" before it finishes.

A finding looks like this:

```
[HIGH] src/webhooks.js:41  (core.webhook_signature_unverified)
Webhook handler accepts requests without verifying the provider's signature.
Anyone who finds the URL can forge events.

How to fix:
Verify the signature on the RAW request body before parsing or acting on it …
```

The agent fixes it, then asks Tori to verify the fix against the file. Verified fixes are what count.

## Deferring a fix

If you deliberately don't want to fix something yet, tell the agent — it records a waiver with your reason. Waived findings stop blocking, stay visible, and never earn anything. You can also put a comment on the line above the flagged code:

```js
// tori-ignore: logging-only stub, verification comes with the real handler
```

Remove the comment and the finding comes back.

## Rewards

When you build with Tori you're rewarded in AI credits. Credits become a spend-capped API key you can use in Cursor, Claude Code, or anything OpenAI-compatible. You'll get an email the first time you're rewarded, then a monthly statement — never an email per reward. What earns and how much isn't published.

## Commands and tools

| CLI | |
|---|---|
| `npx @earntori/cli login` | sign in with GitHub |
| `npx @earntori/cli init` | wire Tori into the current repo (MCP server + Claude Code hooks) |
| `npx @earntori/cli check [paths…]` | run the checks by hand on changed files |
| `npx @earntori/cli status` | balance and your AI keys |

| MCP tools (what the agent calls) | |
|---|---|
| `checks_run` | check changed files, get findings with fixes |
| `checks_resolve` | verify a fix against the file's current content |
| `checks_waive` | record a deliberate deferral, with a reason |
| `status` | open, waived and resolved findings |

## What gets sent where

The rules run on Tori's servers. When a check runs, the changed code files are sent over your signed-in session and evaluated; findings are stored so fixes can be verified and so they stay out of your way once waived. Nothing runs in the background on your machine — the only local files are `.mcp.json`, `.cursor/mcp.json` and `.claude/settings.json` in the repo, plus `~/.tori/config.json` for your login. Delete them and Tori is gone.

## Requirements

- A GitHub account (sign-in is GitHub's device flow)
- Claude Code or Cursor
- A repo with `git init` (Tori uses git to find changed files)
- JavaScript / TypeScript projects today

## Beta

This is a beta. Tell us what it flags, what it misses, your wishlist, and what annoys you — open an issue here.

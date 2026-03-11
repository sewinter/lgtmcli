# LGTM

```
curl -fsSL https://releases.lgtmcli.dev/install.sh | sh
```

You context-switch to a browser tab. You skim a 400-line diff. You leave only an LGTM. You context-switch back and try to remember what you were doing. Code review shouldn't break your flow.

**LGTM is a terminal-native code review tool.** It syncs your PRs from GitHub, compiles an AI briefing for each one (risk assessment, flagged areas, draft inline comments) and lets you review and submit without leaving the terminal.

## What a briefing looks like

```
supabase/supabase#19284: Add billing usage export endpoint
author: billing-dev | opened: 2h ago | CI: pass

What changed:
  • New GET endpoint for billing usage CSV export
  • CSV formatting helper for usage data

Risk: high
  New route handles sensitive billing data but is missing auth middleware.
Suggested action: request changes | Files: 3 | Lines: +78/-3

What matters:
──────────────────────────────────────────────────
[critical] Missing authentication on billing export route
           src/routes/index.ts:30

  30 +  app.get('/api/v1/organizations/:orgId/billing/export', exportHandler);

Every other route in this file uses requireAuth middleware. This endpoint
exposes organization billing data without authentication.

3 drafts prepared. Run `lgtm review` to walk through.
```

`lgtm review` walks you through its draft comments — edit, accept, or discard each one — and `lgtm submit` pushes your review to GitHub.

## It learns your codebase

Run `lgtm init` on a repo and LGTM reads your team's past review comments to bootstrap conventions — patterns like "we always wrap DB calls in transactions" or "errors in this service must use our custom error type." These conventions are checked on every future PR. You can also write them by hand in `LGTM.md` or `.lgtm/conventions.yml`. Over time, it catches the stuff your team cares about without being told.

## Get started

```bash
# 1. GitHub auth — if you have the gh CLI, you're already set.
#    Otherwise:
lgtm config --github-token ghp_...

# 2. Anthropic API key (BYOK — runs on your machine, calls Anthropic directly):
lgtm config --api-key sk-ant-...

# 3. Verify:
lgtm config
```

## Usage

Just run `lgtm`. It syncs your PRs, shows your queue, and walks you through each review (briefing, diff, draft comments, submit) all with single-key navigation.

```bash
lgtm                        # start here
```

Every screen is also a standalone command:

```bash
lgtm sync                   # pull PRs assigned to you
lgtm brief 1                # read the AI briefing for PR #1
lgtm diff 1                 # view the diff with AI annotations
lgtm review 1               # stage inline comments + summary
lgtm submit 1               # push your review to GitHub

# Review any PR (even if you're not assigned):
lgtm brief owner/repo#123
```

## Requirements

macOS (Apple Silicon or Intel) or Linux x86_64 and an [Anthropic API key](https://console.anthropic.com/). Typical cost is a few cents per PR briefing.

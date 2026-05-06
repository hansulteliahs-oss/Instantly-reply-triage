# Instantly Reply Triage — Cloud Routine

This repo holds the canonical prompt for the **Instantly Reply Triage** Claude Code routine that runs on Anthropic cloud infrastructure (`claude.ai/code/routines`).

## What this routine does

A new cold-email reply lands in one of the 4 Handled outbound inboxes (`eli@`/`eliahs@` on `handledlab.com`/`handledbase.com`). The Instantly webhook fires an n8n catcher, which POSTs to this routine's `/fire` endpoint. The routine then:

1. Pulls lead context from Airtable
2. Researches the lead via Apollo + Tavily (parallel, fail-soft)
3. Classifies the reply (`interested` / `question` / `polite_no` / `ooo` / `unsubscribe`)
4. Drafts a tailored response in Eliahs's voice
5. Saves the Gmail draft in the original thread
6. Upserts Airtable (`Leads for Instantly Campaigns` + `Reply Log`)
7. Fires an ntfy push if hot

Never auto-sends. Eliahs reviews in Gmail and hits Send.

## Files

- **`ROUTINE_PROMPT.md`** — canonical prompt. The routine reads this on every fire. Edit here, `git push`, next fire picks up the change.

## How the routine reads this repo

The routine's UI Prompt field is a thin shim that says: clone this repo, read `ROUTINE_PROMPT.md`, follow it exactly. The full prompt body lives in the file.

## Editing flow

1. Edit `ROUTINE_PROMPT.md`
2. `git commit -am "describe the change"`
3. `git push`
4. Next routine fire reads the new version

## Related

- Plan: `~/.claude/plans/scalable-roaming-pelican.md`
- Skill: `~/AI Operating System/.claude/skills/instantly/SKILL.md` § "Reply triage"
- Decisions: `~/AI Operating System/decisions/log.md` 2026-05-05 entry

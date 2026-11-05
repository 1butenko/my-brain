---
title: Contract workflow agents
owner: Ihor Butenko
status: draft
date: 2026-11-04
type: tech
---

# Contract workflow agents

Three subagents turn one contract PDF into a HubSpot deal, or into an exception for a person. Run them with `/process-contract contracts/<name>.pdf`. The rules are in [`rules/escalation.md`](../rules/escalation.md).

Why three agents and not one: the verifier checks the PDF without seeing the extractor's reasoning, so a misread has to happen twice, independently, to slip through. The gatekeeper only applies rules, so its decision can be checked against the rules file.

| Agent | What it does | What it touches | What it can never do | Owner |
|---|---|---|---|---|
| `extractor` | Reads the PDF and returns the deal fields as JSON, each with a page and an exact quote. Missing means `null`. | The contract PDF (Read only) | Write files, call HubSpot or Slack, guess a value, read contract 03 | Ihor Butenko |
| `verifier` | Gets the PDF path and the extractor's values only. Checks each one against the PDF: match, mismatch or not_found. Reports clauses and dates the extractor missed. | The contract PDF (Read only) | See the extractor's reasoning, write files, call HubSpot or Slack, read contract 03 | Ihor Butenko |
| `gatekeeper` | Applies rules 1 to 7 to the two JSON results and the price list. Returns clean or exception, the rules fired and a one-sentence reason each. | `out/*.json`, `rules/escalation.md`, `reference/price_list.*` (Read, Grep) | Call HubSpot, write files, read the contracts | Ihor Butenko |

## What the main session does

- Checks rule 8 in HubSpot, **read only**.
- Saves `out/<contract>_extractor.json`, `_verifier.json` and `_verdict.json`.
- Clean: shows the deal payload and creates the deal only after Ihor approves that deal in the chat.
- Exception: writes `outbox/slack-drafts/<contract>_<date>.md`. Nothing is sent. Ihor sends it once a test Slack workspace exists.

## Guard rails

- `.claude/settings.json` denies every Slack and Gmail tool, and reading any file in `contracts/` named like 03.
- HubSpot deal creation goes through `manage_crm_objects`, which also edits records, so it isn't allowlisted: every call asks first.
- These rules apply only when Claude Code starts in this repo.

---
title: How to use this brain
owner: Ihor Butenko
status: draft
date: 2026-11-05
type: process
---

# How to use this brain

This repo is the project's shared memory. If you're new, read it in your first week to learn what the business is, who's involved, what was decided and why, and how the work gets done.

It does two jobs:

1. **Notes you can read and ask about**: decisions, meetings, pitch material, the team, operations.
2. **A contract workflow** that turns a sponsorship contract PDF into a HubSpot deal, or into a question for a person.

You can read it as plain Markdown files. It works best opened in [Claude Code](https://claude.com/claude-code), which can answer questions from it and run the contract workflow.

## 1. Get started

```bash
git clone <this repo>
cd my-brain
claude          # start Claude Code here, so the repo's settings and commands load
```

You need Claude Code started **inside this folder**. The permission rules in `.claude/settings.json` and the `/process-contract` command only apply then.

## 2. What's where

| Folder | What's in it |
|---|---|
| `01-why/` | Why the product exists: problem, vision. *Empty for now.* |
| `02-market/` | Market, competitors, customer interviews. *Empty for now.* |
| `03-strategy/` | Strategy. *Empty for now.* |
| `04-roadmap/` | What's planned, in progress, dropped. *Empty for now.* |
| `05-decisions/` | One file per decision: what, when, who, why, alternatives |
| `06-meetings/` | Call and meeting notes: date, attendees, outcomes, action items |
| `07-pitch/` | Line sheets, brand voice, messaging |
| `08-tech/` | Tech docs. *Empty for now.* |
| `09-how-we-work/` | The team, rituals, tools, norms. Start with `team.md` |
| `10-operations/` | Suppliers and partners, fulfillment, freight, insurance |
| `_templates/` | Document templates. *Empty for now.* |
| `agents/` | How the contract workflow's agents work |
| `rules/` | `escalation.md`: when a contract needs a person |
| `DIGEST.md` | What changed in the brain in the last 7 days |

These folders are **local only** and never committed (see `.gitignore`):

| Folder | Why it stays local |
|---|---|
| `inbox/` | Raw intake: may hold unsigned contracts, receipts, private emails |
| `contracts/` | Contract PDFs to process |
| `reference/` | The price list (`price_list.pdf`) |
| `out/*.json` | Workflow results; they quote contract text and signatory names |
| `outbox/slack-drafts/*.md` | Exception messages waiting to be sent |

If you clone the repo, these will be empty or missing. Ask the owner for the files you need.

## 3. Ask questions

In Claude Code, just ask: *"Who is on the team?"*, *"What did we decide about the large serving bowl?"*, *"What was the last call with Marcus?"*

Answers must come **only from the files in this repo**. If something isn't written down here, the answer is "not in the brain", not a guess. If you find a gap, that's a sign a doc is missing: write it (section 4).

## 4. Add a document

1. Put it in the right folder (table above). Operations content goes in `10-operations/`, not `09-how-we-work/`.
2. Name it `YYYY-MM-DD-short-topic.md` for dated things (decisions, meetings), or `topic.md` for living docs.
3. Start it with this front-matter:

   ```yaml
   ---
   title: <title>
   owner: <one person, never "the team">
   status: draft | final | signed | sent
   date: YYYY-MM-DD
   type: decision | meeting-notes | roadmap | pitch | tech | process | other
   ---
   ```

4. Say where it came from on a `**Source:**` line (for example `notes from call.docx`, inbox).
5. Write the **why**, not only the what. A decision without its reasoning is hard to revisit.

### Never put these in

- Passwords, API keys, tokens or any credentials
- Personal data about anyone: health, finances, private chats, personal contact details, card numbers
- Unsigned contracts or drafts still being negotiated
- Anything shared in confidence

If a source contains any of these, leave that part out and note what you left out. Policy numbers and individual phone numbers and emails are already kept out on purpose. Only company-level contacts are listed.

## 5. When two docs disagree

Go down this list and stop at the first rule that settles it:

1. **Signed or sent beats draft.**
2. **A decision doc beats meeting notes.**
3. **Final beats non-final.** If several are final, the newest `date` wins.
4. **Still unclear? Ask the owner.** Don't pick a winner yourself. Once they decide, record it as a decision doc in `05-decisions/`.

When you answer from the brain and hit a conflict, say which file you used and which rule settled it.

## 6. Who owns what

Every area has one named owner. The owner keeps it accurate and decides conflicts.

| Area | Owner |
|---|---|
| Everything (product, pitch, tech, meetings, how we work, operations) | Ihor Butenko |

Right now one person maintains the brain. When people join, give each area one owner and update this table.

## 7. Process a contract

This turns a sponsorship contract PDF into a HubSpot deal, or into a question for a person when something looks off.

**You need:**

- The contract PDF in `contracts/`
- The price list at `reference/price_list.pdf`
- The HubSpot connector in Claude Code, signed in to the right account

**Run:**

```
/process-contract contracts/<name>.pdf
```

**What happens:**

1. **`extractor`** reads the PDF and returns each deal field (brand, amount, dates, deliverables, special clauses) with a page number and an exact quote. A missing value stays empty; it never guesses.
2. **`verifier`** checks each value against the PDF on its own, without seeing how the extractor got there. A misread has to happen twice to slip through.
3. **`gatekeeper`** applies rules 1–7 from `rules/escalation.md` using the two results and the price list.
4. **Rule 8** is checked in HubSpot (search only): does this brand already have an open deal or a near-identical company?
5. Results are saved to `out/<name>_extractor.json`, `_verifier.json` and `_verdict.json`.

**Then one of two things:**

- **Clean.** You're shown the exact deal. It is created in HubSpot **only after you say yes to that specific deal.** Nothing existing is ever edited or deleted.
- **Exception.** A message is written to `outbox/slack-drafts/<name>_<date>.md` with a table of what's wrong and one question. **Nothing is sent.** A person reads it and sends it.

**The 8 escalation rules**, in short (full text with examples in `rules/escalation.md`):

| # | Escalate when |
|---|---|
| 1 | A required field is missing or unreadable |
| 2 | Extractor and verifier disagree |
| 3 | The amount is below the price list |
| 4 | The deliverables match no standard package |
| 5 | A clause or deliverable is outside the standard package (exclusivity, custom deliverables, etc.) |
| 6 | Dates look wrong |
| 7 | The contract is unsigned or marked draft |
| 8 | HubSpot already has this brand |

When in doubt, the workflow escalates: asking a person is cheaper than fixing a wrong number in the CRM.

### Guard rails

- Slack and Gmail tools are **denied** in `.claude/settings.json`. The workflow can't read or send messages.
- Creating a HubSpot deal always asks for permission first.
- No emails or messages go to anyone outside.

## 8. Quick checklist for newcomers

- [ ] Read `09-how-we-work/team.md` and `10-operations/partners.md`
- [ ] Skim `05-decisions/` and `06-meetings/`
- [ ] Check `DIGEST.md` for what changed this week
- [ ] Ask Claude Code anything; trust "not in the brain" over a guess
- [ ] Before processing contracts, get the price list and HubSpot access from the owner

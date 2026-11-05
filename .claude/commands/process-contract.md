---
description: Run the contract-to-HubSpot pipeline (extractor, verifier, gatekeeper, rule 8, routing) for one contract PDF.
argument-hint: contracts/<name>.pdf
---

Process the contract at `$ARGUMENTS`. Follow the limits in the "Contract workflow" section of `CLAUDE.md`. Rules live in `rules/escalation.md`.

## 0. Checks (stop on any failure)

- If the path or file name contains `03`, stop and say: "Contract 03 is the holdout. Not processed." Don't open it.
- If the PDF doesn't exist, or `reference/price_list.*` doesn't exist, say which is missing and stop.
- Never call any Slack or Gmail tool. Never send anything to anyone.
- `<contract>` below is the file name without extension, e.g. `01`. The run date is today.

## 1. Extract

Run the `extractor` subagent with only the PDF path. Save its JSON, unchanged, to `out/<contract>_extractor.json`.

## 2. Verify

Run the `verifier` subagent with the PDF path and **only**:
- a plain list of `field: value` pairs taken from the extractor JSON
- one line per `extra_clauses` entry: `type @ clause_ref (page N)`, plus `grants` or `denies` for exclusivity, e.g. `exclusivity (denies) @ 6.1 (page 2)`

Don't pass quotes, reasoning or any summary. The clause reference says where to look; the verifier judges the label itself. Save its JSON to `out/<contract>_verifier.json`.

## 3. Gatekeeper (rules 1 to 7)

Run the `gatekeeper` subagent with the two JSON paths and the run date. Keep its JSON.

## 4. Rule 8 (main session, HubSpot read only)

- Say which HubSpot account is connected (`get_user_details` / `get_organization_details`).
- Search HubSpot deals and companies for the brand and near matches (case, punctuation, Inc/LLC/Ltd/GmbH, one or two characters off). Use only search and read tools.
- If an open deal or a near-identical company exists, add a rule 8 entry to `rules_fired` and set `verdict` to `exception`.
- If `brand` is null, rule 1 has already fired; skip the search and say so.

Save the final verdict to `out/<contract>_verdict.json`.

## 5. Show the result

Show a table: field | extractor value | verifier result | PDF page. Then list the rules fired, and every place the verifier disagreed with the extractor or reported something unreported.

## 6. Route

**Clean:** show the exact deal payload: deal name, amount, close date, pipeline, stage, company, and the HubSpot account. Any value you can't take from the contract or from HubSpot stays empty, and you ask me. Ask "Create this deal?" and stop. Create it only after I say yes to this specific deal, then report the record link. Never edit, overwrite or delete an existing HubSpot record.

**Exception:** write `outbox/slack-drafts/<contract>_<YYYY-MM-DD>.md` with this content, and do nothing else:

```markdown
---
title: Contract <contract>: exception
owner: Ihor Butenko
status: draft
date: <YYYY-MM-DD>
type: other
---

**Contract <contract> needs a decision.** Not sent: draft only.

| Rule | Field | Extractor | Verifier | PDF page |
|---|---|---|---|---|
| <n> | <field> | <value> | <result / pdf value> | <page> |

**Question:** <one line the person can answer>
```

No personal data beyond the brand name goes in the draft.

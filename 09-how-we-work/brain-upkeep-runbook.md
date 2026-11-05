---
title: Brain upkeep, 15-minute weekly runbook
owner: Ihor Butenko
status: draft
date: 2026-11-05
reviewed: 2026-11-05
type: process
---

# Brain upkeep: 15-minute weekly runbook

**Shared doc:** https://claude.ai/artifact/P6QHVrhMJsr3iGFLjyTZis

## Why

Every Monday, spend 15 minutes on the brain so it stays a source people can trust. A brain that is out of date is worse than an empty one: answers come only from what is written, so a stale fact turns into a wrong answer.

The routine follows the brain's own rules:

- Every doc has front-matter: `title`, `owner`, `status`, `date`, `type`.
- Write the why, not only the what.
- Never add credentials, personal data, unsigned contracts, or anything shared in confidence.
- When docs disagree: signed or sent beats draft, a decision doc beats meeting notes, then the newest final doc wins. Anything still unclear goes to the owner.

**Owner:** Ihor Butenko. **When:** Mondays, starting 9 Nov 2026.

## The Monday routine (15 min)

Five steps, in this order. If one runs over, stop and carry it to next Monday instead of going past 15 minutes.

| Minutes | Step | What you do | Where |
|---|---|---|---|
| 0–2 | See what changed | Read `DIGEST.md` and the last 7 days of `git log`. List any new files in `inbox/`. | `DIGEST.md`, `inbox/` |
| 2–7 | Add new decisions and meetings | Write up each decision or call from last week as its own file, with front-matter and the why. Then clear those source files from `inbox/`. | `05-decisions/`, `06-meetings/` |
| 7–11 | Check docs older than 30 days | Run the stale-doc check (Commands below). Open each doc it lists. If it's still true, set `reviewed:` to today. If it isn't, go to the next step. | `07-pitch/`, `09-how-we-work/`, `10-operations/`, `rules/`, `agents/` |
| 11–14 | Remove what's no longer true | Fix the fact in place and note what changed and why. If a decision reversed an older one, write a new decision doc and link back. Never delete a decision or meeting note. Tick off finished action items. | Any doc the check flagged |
| 14–15 | Commit and push | Commit with a one-line message, for example `Weekly brain upkeep 2026-11-09`, and push. | git |

**Records don't go stale.** Decisions and meeting notes describe what happened on their date, so they are never edited to match the present. A newer decision replaces them instead. The 30-day check therefore skips `05-decisions/` and `06-meetings/`.

**Add a `reviewed:` field.** In living docs (team, partners, operations, pitch, rules), `date` is the source's date, for example when a contract was signed. Add `reviewed: YYYY-MM-DD` under it so the check measures when someone last confirmed the doc, not how old the source is.

## 30-day plan

The first four Mondays each run the routine above, plus one focus task. The goal: by 30 Nov every living doc has been checked, every open loop has an outcome, and the routine fits in 15 minutes.

### Week 1, 9 Nov 2026: set the baseline

- [ ] Add `reviewed:` to the front-matter of every living doc
- [ ] Review the 4 docs the check flags today: `10-operations/insurance.md` (Aug 2025 certificate), `10-operations/fulfillment-tidewater.md`, `10-operations/freight-northgate.md`, and `07-pitch/2026-spring-wholesale-line-sheet.md`
- [ ] Settle `CLAUDE.md`: it is deleted locally, but `/process-contract` still points to it. Restore it, or move its rules somewhere Claude Code loads

### Week 2, 16 Nov 2026: close open loops

- [ ] Marcus: what we actually owe Kiln & Co, and the revised cost sheet (from the 12 May call)
- [ ] Dana: was Fern & Stone told the large serving bowl is being dropped?
- [ ] Second kiln for mugs: decide, and record it as a decision doc
- [ ] Two unsent exception drafts in `outbox/slack-drafts/`: contract 02 (Quanta Ledger) and Orbis Payments. Record each outcome

### Week 3, 23 Nov 2026: finish the drafts

- [ ] Move to final, or note what's still missing: `07-pitch/brand-voice.md`, `rules/escalation.md`, `agents/README.md`, `README.md`
- [ ] Add a front-matter template to `_templates/`

### Week 4, 30 Nov 2026: fill a gap and review

- [ ] Write one page in `01-why/`. The brain doesn't yet say what the business is
- [ ] Retro: did each Monday fit in 15 minutes? Adjust this runbook and set the next 30-day focus

## Commands

Run these from the repo root on macOS. They only read; nothing changes until you edit and commit.

**What changed in the last 7 days:**

```bash
git log --since="7 days ago" --name-status --date=short --format='%h %ad %s'
ls -lt inbox/
```

**Living docs not checked in 30 days** (uses `reviewed:`, falls back to `date:`, skips decisions and meetings):

```bash
cutoff=$(date -v-30d +%F)
git ls-files '*.md' | grep -vE '^(05-decisions|06-meetings)/' | while read -r f; do
  [ -f "$f" ] || continue
  d=$(sed -n 's/^reviewed: //p' "$f" | head -1)
  [ -z "$d" ] && d=$(sed -n 's/^date: //p' "$f" | head -1)
  [ -n "$d" ] && [[ "$d" < "$cutoff" ]] && echo "$d  $f"
done | sort
```

**Drafts still open:**

```bash
grep -l '^status: draft' $(git ls-files '*.md') 2>/dev/null
```

**Finish:**

```bash
git status --short        # look first: commit only this week's upkeep
git add <the files you changed> && git commit -m "Weekly brain upkeep $(date +%F)" && git push
```

In Claude Code, you can also ask: *"Run the Monday brain upkeep."*

## Done, and when stuck

A Monday is done when:

- every decision and call from last week is written up
- every doc the check flagged is either confirmed (`reviewed:` set to today) or fixed
- the changes are pushed

The 30 days are done when the check comes back empty, no action item is older than 30 days without an outcome, and three Mondays in a row fit in 15 minutes.

**When stuck:**

| Situation | Do this |
|---|---|
| Two docs disagree | Apply the conflict rules. If they don't settle it, ask the owner and record the answer as a decision doc |
| You can't tell if a fact is still true | Ask the person who owns that area. Until they answer, add a one-line open question to the doc |
| A source holds personal data, credentials or an unsigned contract | Leave that part out and note what you left out |
| The routine runs past 15 minutes | Stop. Carry the rest to next Monday and note it in the commit message |

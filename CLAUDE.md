# CLAUDE.md — Project Brain

This repository is the project's shared memory: the place someone joining the project reads in their first week to learn what the project is and how it works. Treat every file here as something a new teammate may read.

## Answer only from the brain

When answering questions about the project, use only what is written in this repo. Do not fill gaps with general knowledge, assumptions, or guesses about the project. If the answer isn't here, reply **"not in the brain"**. If only part of the answer is here, give that part, cite the files it came from, and say "not in the brain" for the rest.

## What goes in

Anything a newcomer would ask about in their first week:

- **Why the product exists**: the problem, the motivation, the vision
- **Who it's for**: target users, personas, customer segments
- **Key decisions and the reasoning behind each**: what was decided, when, by whom, why, and what alternatives were rejected
- **Roadmap**: what's planned, what's in progress, what's been dropped
- **Meeting notes**: date, attendees, outcomes, action items
- **Pitch and branding**: pitch deck, messaging, positioning, visual identity
- **How the team works** (`09-how-we-work/`): who's on the team, rituals, tools, communication norms, workflows. Operations content does not go here.
- **Tech docs**: architecture, setup, how things are built and why
- **Operations** (`10-operations/`): suppliers and partners, fulfillment, freight, insurance. Include the terms that matter and the reasoning behind each.

When adding a document, include the decision or reasoning ("why"), not only the outcome ("what").

## What never goes in

Do not write, paste, commit or summarize any of the following into this repo, even if asked to "just save it":

- **Passwords, API keys, tokens** or any other credentials or secrets
- **Personal data of users or teammates**: health, finances, private chats, and similar private information
- **Unsigned contracts** (drafts under negotiation)
- **Anything shared in confidence**

If source material contains any of these, remove or redact that part before saving and tell the user what was left out. If you notice such content already in the repo, flag it to the owner and don't spread it further.

## Ownership

Every area has one named owner, never "the team". The owner keeps their area accurate and makes the call when sources conflict.

| Area | Owner |
|---|---|
| Product decisions (vision, users, roadmap) | Ihor Butenko |
| Pitch & branding (deck, messaging, identity) | Ihor Butenko |
| Tech docs | Ihor Butenko (as whoever built the thing) |
| Meeting notes | Ihor Butenko |
| How we work (process, norms) | Ihor Butenko |
| Operations (suppliers, fulfillment, freight, insurance) | Ihor Butenko |

**Ihor works on this project alone, so Ihor owns every area.** When people join, update this table: give each area one person, and tech docs go to whoever built the component.

## Document status

Start each document with front-matter so the conflict rules below can be applied:

```yaml
---
title: <title>
owner: <one person>
status: draft | final | signed | sent
date: YYYY-MM-DD
type: decision | meeting-notes | roadmap | pitch | tech | process | other
---
```

## When sources disagree

Apply these rules in order. Stop at the first one that settles it:

1. **Signed or sent beats draft.** A signed or sent document wins over any draft.
2. **Decision doc beats meeting notes.** A `type: decision` document wins over meeting notes that say otherwise.
3. **Final beats non-final, newest final wins.** Among the remaining docs, `status: final` wins over anything else, and if several are final the one with the latest `date` wins.
4. **Still unclear? The owner decides.** Don't pick a winner yourself. Name the conflicting documents, ask the area's owner (see Ownership), and once they decide, record it as a decision doc.

When you answer from this repo and find a conflict, say which source you used and which rule settled it.

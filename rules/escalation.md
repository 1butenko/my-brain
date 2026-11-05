---
title: Contract escalation rules
owner: Ihor Butenko
status: draft
date: 2026-11-04
type: process
---

# Contract escalation rules

A person decides on a contract if **any** rule below is true. A deal is clean only if none is true.

Rules 1 to 7 are decided by the `gatekeeper` subagent from the extractor and verifier JSON. Rule 8 is checked by the main session in HubSpot, read only.

Why these rules exist: a deal created from a misread contract puts a wrong number into the CRM, and it's cheaper to ask a person than to fix it later. When in doubt, escalate.

## 1. Required field missing or unreadable

- **Condition:** any required field is `null` in the extractor output, or the verifier returns `not_found` for it.
- **Fields:** `brand`, `amount`, `currency`, `deliverables`, `start_date`, `end_date`, `payment_terms`.
- **Example:** the contract says "Fee: see Schedule B", but Schedule B isn't in the PDF, so `amount` is `null`.

## 2. Extractor and verifier disagree

- **Condition:** any of these is true:
  - the verifier returns `mismatch` for any field
  - the verifier returns `wrong_type`, `wrong_effect` or `not_found` for any claimed clause: the two agents disagree about what a clause is or does
  - `amount_in_words` doesn't equal `amount` in numbers
- **Fields:** every extracted field, every entry in `extra_clauses`, plus `amount_in_words` against `amount`.
- **Example:** page 2 says "$12,000 (ten thousand US dollars)", so the amount in words differs from the number. Or: the extractor labels clause 6.1 `exclusivity`, but the clause says no exclusivity is granted, so the verifier returns `wrong_effect`.
- **Why it changed (2026-11-05):** the verifier first got only clause types, with no location, so it could confirm a clause existed but not challenge its label. On contract 01 it missed a "no exclusivity" clause labelled exclusivity and standard items labelled custom.
- **Changed again (2026-11-05):** with label checks on, contract 01 stopped on rule 2 six times, each time because the extractor labelled something it couldn't judge (standard versus custom needs the price list). The extractor now labels only the four rule 5 topics, says whether exclusivity is granted or denied, has no `other` type, and folds survival terms into their parent clause. Custom versus standard is decided only by the gatekeeper.

## 3. Amount below the price list

- **Condition:** `amount` is lower than the price-list price for the matched package, in the same currency. A different currency from the price list also escalates, because converting means guessing a rate.
- **Fields:** `amount`, `currency`, `deliverables` (to find the package), and `reference/price_list.*`.
- **Example:** the deliverables match the "Standard" package, priced at $5,000, but the contract amount is $4,200.

## 4. Deliverables match no standard package

- **Condition:** the `deliverables` list doesn't equal the contents of any package in the price list.
- **Field:** `deliverables`.
- **Example:** the contract asks for 3 Reels and 1 podcast mention, and no package has a podcast mention.

## 5. Clause outside the standard package

- **Condition:** the contract, judged on the quoted text and not on the type label, contains any of these:
  - **exclusivity or category protection** that the contract actually grants. A clause saying no exclusivity is granted doesn't count, because the standard terms say the same.
  - **usage rights** beyond performing the agreement and promoting the event, for example use of the other party's content or brand after the term, in paid ads, or with the right to sublicense. A mutual, non-exclusive name and logo licence limited to performing the agreement or promoting the event is standard and doesn't count.
  - a **perpetual license**, of any scope.
  - **extra revision rounds**.
  - a **custom deliverable**: any deliverable that isn't in the matched price-list package. Only the gatekeeper decides this, by comparing the deliverables with the price list. The extractor and verifier don't label deliverables.
- **Fields:** `extra_clauses`, `deliverables`, the verifier's `unreported`, and the price list.
- **Example:** page 4 says "Creator grants Brand a perpetual, worldwide license to use the content". Counter-example: "Each Party grants the other a non-exclusive, royalty-free licence to use its name and logo solely to perform this Agreement" doesn't fire.
- **Why it changed (2026-11-05):** the first runs on contracts 01 and 02 fired on clause labels alone: a clause denying exclusivity, a standard mutual logo licence, and Gold-package items labelled custom. That buried the clauses that actually matter.

## 6. Dates look wrong

- **Condition:** any of these is true:
  - `end_date` is before `start_date`
  - `end_date` is before the run date: the contract has already ended
  - a date is written in an ambiguous format where day and month can both be 12 or less, such as `03/04/2026`
- **Fields:** `start_date`, `end_date` (judged on the exact quote, not the normalised value).
- **Example:** "Campaign starts 03/04/2026" could be 3 April or 4 March.
- **Why it changed (2026-11-05):** the first version fired when `start_date` was before the run date. Contracts are processed after signing, so that was true for almost every contract, including 01 and 02. An already-ended contract is the case that's really wrong.

## 7. Unsigned or marked draft

- **Condition:** `signed` is `no` or `unclear`, or `draft_marker` is `yes`.
- **Fields:** `signed`, `draft_marker`.
- **Example:** the signature block is empty, or the header says "DRAFT v2".

## 8. HubSpot already has this brand

- **Condition:** HubSpot has an open deal for this brand, or a company whose name is identical or near-identical: differing only in case, punctuation, spacing, or suffixes such as Inc, LLC, Ltd or GmbH, or by one or two characters.
- **Field:** `brand`, searched in HubSpot deals and companies (read only).
- **Example:** the contract brand is "Northwind Co." and HubSpot has a company "Northwind Co" with an open deal.

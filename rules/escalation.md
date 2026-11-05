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

- **Condition:** the verifier returns `mismatch` for any field, or `amount_in_words` doesn't equal `amount` in numbers.
- **Fields:** every extracted field, plus `amount_in_words` against `amount`.
- **Example:** page 2 says "$12,000 (ten thousand US dollars)", so the amount in words differs from the number.

## 3. Amount below the price list

- **Condition:** `amount` is lower than the price-list price for the matched package, in the same currency. A different currency from the price list also escalates, because converting means guessing a rate.
- **Fields:** `amount`, `currency`, `deliverables` (to find the package), and `reference/price_list.*`.
- **Example:** the deliverables match the "Standard" package, priced at $5,000, but the contract amount is $4,200.

## 4. Deliverables match no standard package

- **Condition:** the `deliverables` list doesn't equal the contents of any package in the price list.
- **Field:** `deliverables`.
- **Example:** the contract asks for 3 Reels and 1 podcast mention, and no package has a podcast mention.

## 5. Clause outside the standard package

- **Condition:** the extractor's `extra_clauses`, or the verifier's unreported findings, include exclusivity, usage rights, a perpetual license, extra revision rounds, or a custom deliverable.
- **Fields:** `extra_clauses`, and the verifier's `unreported`.
- **Example:** page 4 says "Creator grants Brand a perpetual, worldwide license to use the content".

## 6. Dates look wrong

- **Condition:** any of these is true:
  - `end_date` is before `start_date`
  - `start_date` is before the run date
  - a date is written in an ambiguous format where day and month can both be 12 or less, such as `03/04/2026`
- **Fields:** `start_date`, `end_date` (judged on the exact quote, not the normalised value).
- **Example:** "Campaign starts 03/04/2026" could be 3 April or 4 March.

## 7. Unsigned or marked draft

- **Condition:** `signed` is `no` or `unclear`, or `draft_marker` is `yes`.
- **Fields:** `signed`, `draft_marker`.
- **Example:** the signature block is empty, or the header says "DRAFT v2".

## 8. HubSpot already has this brand

- **Condition:** HubSpot has an open deal for this brand, or a company whose name is identical or near-identical: differing only in case, punctuation, spacing, or suffixes such as Inc, LLC, Ltd or GmbH, or by one or two characters.
- **Field:** `brand`, searched in HubSpot deals and companies (read only).
- **Example:** the contract brand is "Northwind Co." and HubSpot has a company "Northwind Co" with an open deal.

---
title: Brain intake report, founder-q3 inbox
owner: Ihor Butenko
status: final
date: 2026-11-03
type: other
---

# Brain intake report: founder-q3 inbox

This report covers the processing of the `inbox/founder-q3/` folder (14 files) into the brain, and the re-sort that followed. It lists what is where now, what was left out and why, and what still needs your decision.

## 1. What is where now

| Folder | File | What it holds | Status | Source in inbox |
|---|---|---|---|---|
| `05-decisions/` | [2026-05-12-drop-large-serving-bowl.md](05-decisions/2026-05-12-drop-large-serving-bowl.md) | Decision to stop selling the large serving bowl (STW-BWL-L) after sell-through | final | `notes from call.docx` |
| `06-meetings/` | [2026-05-12-kiln-and-co-price-increase.md](06-meetings/2026-05-12-kiln-and-co-price-increase.md) | Kiln & Co call: 14% stoneware price increase from the July 2026 run, action items | final | `notes from call.docx` |
| `07-pitch/` | [2026-spring-wholesale-line-sheet.md](07-pitch/2026-spring-wholesale-line-sheet.md) | The March line sheet: terms, SKUs, prices. Marked out of date for the large bowl | sent | `output.pdf` |
| `07-pitch/` | [brand-voice.md](07-pitch/brand-voice.md) | Brand voice guide, unfinished | draft | `dana_temp.docx` |
| `09-how-we-work/` | [team.md](09-how-we-work/team.md) | Who is on the team and their roles | final | `list.docx` |
| `10-operations/` | [fulfillment-tidewater.md](10-operations/fulfillment-tidewater.md) | Signed Tidewater 3PL agreement: fees, renewal, liability | signed | `Copy of Copy of terms.pdf` |
| `10-operations/` | [freight-northgate.md](10-operations/freight-northgate.md) | Northgate Freight LTL quote and claims rules | final | `2026 rates.pdf` |
| `10-operations/` | [insurance.md](10-operations/insurance.md) | Liability and property insurance, gaps in cover | final | `asdf.pdf` |
| `10-operations/` | [partners.md](10-operations/partners.md) | Suppliers, partners, wholesale accounts, Fern & Stone NDA status | final | `list.docx` + the above |

### Why the re-sort

The folder template came from a software-startup Notion hub and had no place for operations. Supplier, fulfillment, freight and insurance docs had first landed in `09-how-we-work/`. They now live in a new `10-operations/` folder, and the team roster was split out of the partners doc into `09-how-we-work/team.md`. `CLAUDE.md` was updated so the "What goes in" list and the Ownership table both include Operations.

Folders that are still empty: `01-why/`, `02-market/`, `03-strategy/`, `04-roadmap/`, `08-tech/`, `_templates/`. Nothing in the inbox belonged there.

## 2. What was left out, and why

The raw inbox is **not committed**. `inbox/` is in `.gitignore`, so the original files stay only on the local machine. Only the write-ups above are in the repo.

These were left out entirely under the "What never goes in" rules in `CLAUDE.md`:

| File(s) | What it is | Why it's out |
|---|---|---|
| `doc1 (1).docx` | Mutual NDA with Fern & Stone | Not countersigned by us, so it counts as an unsigned contract. Only its status and key terms are noted in [partners.md](10-operations/partners.md). |
| `final_v2 FINAL.docx` | Earlier v2 draft of the Tidewater agreement ($18.00/pallet, $1,400 minimum) | Superseded draft. The signed version wins (rule 1). The draft fees are mentioned once in the Tidewater doc only to say they are **not** in effect. |
| Screenshot, photos, scan (`Screenshot 2026-04-18 at 09.31.22.png`, `IMG_4471.jpg`, `photo.jpg`, `scan_0042.pdf`) | A private email, receipts showing personal card details, and a supplier invoice | Personal data and private correspondence. The only business fact kept is the Apr 18 note that Fern & Stone was waiting on our countersigned NDA. |
| `Untitled spreadsheet.csv` | Inventory export, last counted May 2, 2026 | Stale. Live stock levels come from the Tidewater client portal, so a frozen snapshot in the brain would mislead. |

Also removed from the docs that were kept:

- **Insurance policy numbers**, left out of [insurance.md](10-operations/insurance.md).
- **Individual contacts' direct emails and phone numbers**, left out of [partners.md](10-operations/partners.md). Only company-level addresses are listed.

## 3. What still needs your decision

Most urgent first.

1. **Insurance may have lapsed.** The policy term ended **Aug 31, 2026** and there is no record of the renewal. Confirm it renewed, and send the new certificate so [insurance.md](10-operations/insurance.md) can be updated. Fern & Stone is listed as an additional insured, so they may ask for it too.
2. **Tidewater has probably auto-renewed.** The non-renewal deadline was **Oct 1, 2026**. With no record of notice, the contract likely renewed on Nov 1, 2026 through Oct 31, 2027 at a **$1,850/month minimum** (up from $1,600). Confirm whether notice was sent. If you want out next year, the deadline is Oct 1, 2027.
3. **Fern & Stone NDA.** They signed on Mar 6, 2026 and were still waiting for our countersignature on Apr 18. Was it countersigned? Until it is, they won't share their fall assortment plan or store-level sales.
4. **Fern & Stone and the large bowl.** Dana was to tell them the bowl is being dropped before they built their fall assortment. Was that done? The May 2 inventory export also showed more large bowls committed to orders than on hand. Check the Tidewater portal to see whether that is still true.
5. **Kiln & Co outstanding invoice.** Junho said their accounts team is chasing it. Marcus was to check what we owe. No outcome is recorded.
6. **Northgate freight quote expires Dec 31, 2026.** A 2027 quote is needed.
7. **New line sheet and pricing.** The Spring 2026 line sheet still lists the large bowl, and the 14% Kiln & Co increase applies to all stoneware, not only the bowl. Decide whether wholesale prices change for the next line sheet. Marcus's revised cost sheet isn't in the brain yet.
8. **Second kiln for mugs.** Left open on the May 12 call. Close it or keep it open.
9. **Brand voice.** It's a draft. Decide whether to finish it (Dana's open question was whether it is worth finishing).
10. **Who owns the brain.** `CLAUDE.md` says Ihor works alone and owns every area. The docs, however, name **Dana Okafor as founder** and a team of six, and Dana made the large-bowl decision. Decide who owns which area and update the Ownership table to match. Until then, the conflict rules point every open question to Ihor.
11. **Unclear role.** [team.md](09-how-we-work/team.md) lists Tobi's role as "Customer", copied as-is from the contacts list. Confirm what it should say, for example customer service.
12. **The raw inbox.** The original files are still on disk in `inbox/founder-q3/`, untracked. Decide whether to keep them somewhere safe outside the repo or delete them. The receipts and private email in particular shouldn't sit around.
13. **Merge.** This work is on branch `add-harbor-pine-intake`. `main` still holds only the empty folder structure. Review and merge when you're happy with it.

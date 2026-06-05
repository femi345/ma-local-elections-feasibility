# Local Elections Data Project — Week 1 Feasibility Report

*Prepared by Femi Ositade · June 2026 · for Sarah Hubbard & Darshan Goux*

---

## Bottom line

**It's feasible — more so than I expected.** Across a stratified 12-municipality sample, **almost every target field is publicly available**, and most of it is in machine-readable form. The two municipalities the Lab already has data for (Newton, Amherst) reproduced your existing numbers **exactly** when I re-found them from scratch, which validates the approach.

The honest blockers aren't the core data — they're a few specific, well-understood things: **incumbency is rarely labeled**, **town-meeting attendance is recorded inconsistently**, and a **minority of town sites are scanned-image or bot-protected**. None of these is a dealbreaker; each has a clear handling strategy below.

The single biggest finding: **a lot of this data never needs municipal scraping at all** — the Secretary of the Commonwealth already publishes registration and state/federal results by municipality centrally. That shrinks the hard part of the project substantially.

---

## What I did this week

- Built a **stratified sample of 12 municipalities** from the Lab's Airtable, spanning population **1,931 → 155,929**, both government forms (city/council and open town meeting), 6 counties including the rural west, and the main website platforms.
- Ran a **hybrid investigation**: an availability scan on all 12, plus **deep end-to-end extraction** on 4 (Newton, Amherst, Sutton, Stockbridge) to prove the full find → fetch → extract pipeline.
- Scored each of the **10 target fields** per municipality (🟢 structured / 🟡 buried / 🔴 not found) and captured **41 source screenshots + deep links** so every figure is traceable.
- **Validated against ground truth:** independently re-found all of the Lab's existing Newton and Amherst records — every number matched.

Full detail: **`feasibility-matrix.csv`** (at-a-glance scores) and **`findings-by-muni.md`** (every field, with values, deep links, and embedded source screenshots). Screenshots live in **`screenshots/<municipality>/`**.

---

## Five findings that shape the build

**1. Turnout & registration are solidly available — and already proven.** Every municipality publishes registered-voter counts and local/state/federal turnout, almost always as searchable PDFs or HTML tables. This is the low-risk core.

**2. The state-level shortcut is the headline.** The **Secretary of the Commonwealth** publishes, *by municipality*, in structured downloadable form:
   - **State, federal/presidential, and state-legislative results** → `electionstats.state.ma.us`, **downloadable CSV per election** (1970→present), via one predictable URL pattern.
   - **Registered voters + party enrollment** → `sec.state.ma.us` enrollment statistics (selectable-text PDF, back to ~2004).

   So fields #1, #3, #4 (and the state-legislative/county piece of the ask) can come from **one clean source instead of 351 town websites**. Per-municipality scraping then only needs to cover **local election results, ballot-level detail, and town-meeting turnout.**

**3. Website platforms are concentrated.** Most town sites run **CivicPlus** (DocumentCenter / ArchiveCenter PDFs), with Granicus and a few custom CMSs. A platform-aware extractor built for CivicPlus first would cover the majority of municipalities.

**4. Ballot-level detail is mostly extractable.** Seats on the ballot, unopposed/no-candidate seats, office type, and term length all extract or derive cleanly from results PDFs and warrants — several towns even print term length inline. **Incumbency is the one genuinely hard field** (see below).

**5. Town-meeting attendance is real but uneven.** Of the 7 town-meeting municipalities I sampled, attendance is recorded in 6 — but in different forms: an explicit head-count (Acton, Hingham, Southwick, Stockbridge), a quorum/vote proxy (Sutton, Sturbridge), or not quantified at all (Becket). It exists more often than I feared, but it won't be uniform.

---

## Availability by field (across the 12-muni sample)

| # | Field | Available? | Typical form | Notes |
|---|-------|-----------|--------------|-------|
| 1 | Registered / eligible voters | **12/12** + central | searchable PDF/HTML | also centrally at SoC |
| 2 | Local election turnout | **12/12** | searchable PDF/HTML (1 scanned) | the core local field |
| 3 | State election turnout | **12/12** + central | searchable | 2 town sites gated → use SoC |
| 4 | Federal / presidential turnout | **12/12** + central | searchable | MA folds president into Nov general |
| 5 | Town-meeting attendance | **6/7 towns** | head-count / quorum / proxy | quality varies; 1 not recorded |
| 6 | Special-town-meeting attendance | where held | same as #5 | intermittent events |
| 7 | Seats on ballot | **12/12** | searchable | per-office seat counts explicit |
| 8 | Unopposed / no candidate | **12/12 (derived)** | count candidates vs seats | never labeled, always derivable |
| 9 | **Incumbents running** | **3/12 explicit** | mostly unlabeled | the hardest field — see below |
| 10 | Office type + term length | **12/12 / ~10 explicit** | searchable | term sometimes needs the charter |

---

## Source-traceability (a few examples)

Every figure in the dataset links back to a screenshot of its exact source. Examples:

**Clean structured turnout (Amherst, CivicPlus) — registered & cast highlighted on the official sheet:**
![Amherst 2025 turnout](screenshots/amherst/amherst_turnout_2025_View83342_p1-1.png)

**The hard field, solved (Hingham) — the ballot flags incumbents ("Candidate for Re-election") and prints term length per office:**
![Hingham 2024 ballot](screenshots/hingham/hingham_03_town_election_ballot_2024_terms_incumbents-1.png)

**Town-meeting attendance, recorded explicitly (Acton) — a labeled header line in the meeting abstract:**
![Acton 2024 town meeting attendance](screenshots/acton/acton_02_atm-2024-abstract-attendance-01.png)

---

## The hard parts (and how I'd handle each)

| Challenge | How big | Handling |
|---|---|---|
| **Incumbency (#9)** not labeled | hardest; explicit in only 3/12 | Join each result to the prior cycle's winners (mechanical, multi-document); local news fills gaps. Some towns (Hingham, Southwick, Sturbridge) flag it directly. |
| **Town-meeting attendance (#5/#6)** uneven | recorded in ~6/7 sampled towns, variable form | Extract where present; record the *form* (head-count vs quorum vs proxy) and flag where absent. Accept it won't be 100%. |
| **Scanned-image PDFs** (OCR) | minority — whole sites (Sutton) or odd years (Newton 2023) | OCR fallback in the pipeline; scans are legible, so OCR accuracy should be good. |
| **Bot-gating** (Cloudflare/Akamai/WAF) | several sites (Newton, Sutton, Stockbridge, Sturbridge, Becket) | Direct PDF-asset URLs usually bypass it; the fully-gated case (Stockbridge) needs a headless/real browser. The scraper needs browser-grade fetching, not plain HTTP. |
| **Format drift** year-to-year | real even within one town | Per-document branching (text vs OCR) and tolerant parsing; QA against known values like the calibration I ran. |

---

## Recommended approach (phased)

1. **Harvest the central SoC data first (quick win).** Registration + state/federal/state-legislative results, by municipality, for all 351 — one source, predictable formats. Covers a big share of the ask fast.
2. **Build the CivicPlus-first municipal scraper** for *local* results + ballot detail (seats / unopposed / office type / term). Browser-grade fetching + an OCR fallback + an LLM-assisted parser to turn results PDFs into structured rows. This covers the majority of municipalities.
3. **Add town-meeting attendance extraction** from minutes / annual reports, flagging where it isn't recorded.
4. **Incumbency enrichment** via cross-year roster joins.
5. **QA** every municipality against any known values (the calibration method generalizes).

**Rough effort (AI-assisted):** a solid **v1 covering the "easy ~80%"** — centralized data + searchable-PDF municipalities for the core fields — is realistically a **few weeks of part-time work**. Pushing from ~80% → ~95% coverage (the OCR sites, gated sites, town-meeting attendance, incumbency, format outliers) roughly **doubles** that — it's the classic long tail. I'd suggest scoping to a high-value v1 first and deciding how far into the tail to chase based on what you see.

---

## Open questions for Darshan

1. **Priority fields** — is **town-meeting turnout** (the hardest/most uneven) essential, or nice-to-have? Same question for **incumbency**.
2. **Coverage/accuracy bar** — e.g. "≥90% of municipalities, flag the rest for manual review," or does it need to be comprehensive?
3. **Time range** — strictly since 2020, or grab older years where they're easily available (several sites go back further)?
4. **Output** — should results flow back into the existing Airtable, or into a new dataset (CSV/database)?
5. **Quick win** — want me to start by pulling the **centralized SoC data** for all 351 municipalities? That's high-value and low-risk, and it would sharpen the estimate for the municipal-scraping half.

---

## Where the detail lives

- **`feasibility-matrix.csv`** — 12 municipalities × 10 fields, scored, with access notes.
- **`findings-by-muni.md`** — every field per municipality, with values, deep links, and embedded source screenshots.
- **`screenshots/<municipality>/`** — 41 page-level source screenshots.

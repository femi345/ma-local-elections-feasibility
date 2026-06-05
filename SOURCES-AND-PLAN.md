# Where the data comes from, and the plan

## Two sources

**1. The state website** (Secretary of the Commonwealth). Already has, for every town, as downloadable spreadsheets:
- registered voters
- state + federal election turnout
- state-legislative and county/regional races

We just download these — no town-by-town scraping needed.

**2. The town websites.** Needed only for the *local* pieces:
- local election results
- ballot details (seats, who ran, unopposed/empty, term lengths)
- town-meeting attendance

This is usually 1–2 PDFs per town (the election-results sheet and the annual town report).

## The plan

1. Download the state data first — covers a big chunk for all 351 towns at once.
2. Scrape town sites for the local pieces, starting with the most common website type (covers most towns); add OCR for the few scanned ones.
3. Pull town-meeting attendance from the annual reports, where towns record it.
4. Work out incumbents by comparing each year's candidates to the year before.
5. Spot-check against towns we already have data for.

## A few questions for Darshan

- Are **town-meeting turnout** and **incumbents** must-haves or nice-to-haves? (they're the two hardest)
- What counts as "enough" coverage — e.g. 90% of towns, flag the rest?
- Just 2020 onward, or older years where they're easy to grab?
- Where should the data live — the existing Airtable, or a new spreadsheet?
- Want me to start with the state-data download (the quick win)?

---
*Want the proof? See [findings-by-muni.md](findings-by-muni.md) — every number with its source link and a screenshot.*

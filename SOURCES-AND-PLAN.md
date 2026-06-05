# Local Elections Data — Sources & Plan

A plain map of **where each field comes from** and **how to find it for any town**, plus the **plan forward** and a recommendation on **how to present the data**. Based on the 12-municipality feasibility test (see `REPORT.md` for the verdict and `findings-by-muni.md` for the screenshot-backed detail on each town).

---

## 1. Where each field comes from / how to find it for any town

Two kinds of sources: **🏛️ State (central)** — one website, all 351 towns, downloadable spreadsheets — and **🏘️ Town site** — the individual municipal website.

| Field (from the email) | Primary source | How to find it for any town | Confidence |
|---|---|---|---|
| Registered voters | 🏛️ SoC enrollment statistics (by town) | `sec.state.ma.us` → Elections → Voter Registration/Enrollment. Also on each town's results sheet. | High |
| Local-election turnout | 🏘️ Town Clerk's official results PDF | Town site → "Town/City Clerk" or "Elections" → "Election Results"; tiny towns → Annual Town Report | High |
| State + federal turnout | 🏛️ electionstats.state.ma.us (CSV per election, by town) | Pick the election → "Download this Election" → municipality CSV | High |
| Town-meeting turnout | 🏘️ Town-meeting minutes / Annual Town Report | Town site → "Town Meeting" (warrants/minutes) or Annual Town Report → Town Clerk's report | Medium — varies by town |
| Special-town-meeting turnout | 🏘️ same as above | same | Medium |
| Seats on ballot | 🏘️ Local election results PDF (or ballot/warrant) | The results doc lists each office; "Vote for N" = seat count | High |
| Seats with no one running | 🏘️ same results PDF | An office shown with only blanks/write-ins, no named candidate | High (derived) |
| Seats unopposed | 🏘️ same results PDF | An office with exactly one named candidate | High (derived) |
| Incumbents running | 🏘️ derive across years | Compare each race's candidates to the prior year's winners. A few towns label it ("Inc." column, "Candidate for Re-election", "*"). | Medium — mechanical, multi-doc |
| By office type | 🏘️ same results PDF | Results are already grouped by office (Select Board, School Cmte…) | High |
| Term length per seat | 🏘️ ballot/warrant ("for 3 years"); fallback town charter | Printed next to the office on most towns' ballots/warrants | High–Medium |
| State legislative + county/regional | 🏛️ electionstats.state.ma.us | Same download as state/federal results | High |

**Plain takeaway:** the 🏛️ state source covers registered voters + state/federal/legislative/county results for **all 351 towns at once**. The 🏘️ town sites are only needed for **local results, ballot details, and town-meeting turnout** — and most of those sit in one or two predictable documents per town (the official results PDF and the Annual Town Report).

---

## 2. What we verified (so the numbers are trustworthy)

- **12-town sample**, chosen to span population (1,931 → 155,929), both government forms, multiple website platforms, and 6 counties incl. the rural west.
- **Every "found" field has a screenshot of its exact source + a clickable deep link** (in `findings-by-muni.md` / `screenshots/`). 41 source screenshots total.
- **Method validated against ground truth:** independently re-derived the Lab's existing Newton and Amherst records — every number matched exactly.
- **Honest limits:** this is a sample and a focused look (~20 min/town at the obvious places: Clerk, Elections, Town Meeting, Annual Report), not an exhaustive sweep of every page and year. Where a town is marked "doesn't publish X," that was confirmed by reading the actual document — but treat the overall proportions as a strong indicator, to be confirmed per-town during the real build.

---

## 3. Plan forward

1. **State data first (quick win).** Pull registration + state/federal/legislative/county results for all 351 towns from the SoC. One source, predictable formats — covers a big slice of the list immediately.
2. **Town scraper for the local pieces.** Start with the most common website platform (covers the majority of towns), pulling local results + ballot detail (seats / unopposed / office type / term). Needs browser-grade fetching (a few sites block plain bots) and OCR for the handful of scanned-image towns.
3. **Town-meeting attendance** from minutes / annual reports — extract where present, flag where it isn't recorded.
4. **Incumbency** by comparing each race to the prior year's winners.
5. **QA** every town against any known values (the calibration method we used on Newton/Amherst).

---

## 4. How to present / host the data — recommendation

**Two homes, by audience:**

- **The data the Lab actually uses → a spreadsheet, not code.** Extend the **existing Airtable** — its "Local Elections" table already has the right shape (Municipality, Election date/type, # voters, # eligible, turnout, **Source**). We keep filling that table and add a related "Ballot detail" table for the per-office fields (seats / unopposed / incumbent / office type / term). Crucially, **every row carries a source-URL column** so any number is one click from its source. (A Google Sheet works too, but the Lab already lives in Airtable.)
- **The tool + method → a GitHub repo.** The scraper code, this source guide, and a README. Good for reproducibility and for whoever maintains it later. But researchers shouldn't have to open GitHub to get numbers — **GitHub = code, Airtable = data.**

**For this week's hand-off specifically:** keep it simple — the email + the shared `feasibility/` folder (screenshots + the three writeups) is enough. No need to build anything yet.

---

*Files: `REPORT.md` (the writeup), `feasibility-matrix.csv` (scored grid), `findings-by-muni.md` (every field + source + screenshot), `EMAIL-DRAFT.md` (the reply to Sarah), `screenshots/` (41 source images).*

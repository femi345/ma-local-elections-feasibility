# Massachusetts Local Elections Data — Feasibility (Week 1)

Feasibility test for the Allen Lab Local Election Data Project: can we scrape local-election and voter-turnout data from Massachusetts municipal websites, going back to 2020?

**Verdict: feasible.** I sampled 12 municipalities (big cities down to tiny rural towns), scored each of 10 target fields, and saved a screenshot + source link for every figure. The two towns the Lab already had data for (Newton, Amherst) reproduced exactly.

## Start here
- **[feasibility-matrix.csv](feasibility-matrix.csv)** — the 12 towns × 10 fields at a glance. Each cell: **clean** = ready to scrape · **buried** = there, but in a scanned image (needs OCR) or has to be pieced together · **none** = not published · **n/a** = doesn't apply.
- **[SOURCES-AND-PLAN.md](SOURCES-AND-PLAN.md)** — where the data comes from and the plan (short).
- **[findings-by-muni.md](findings-by-muni.md)** — the proof: every field, its source link, and a screenshot. (For digging deeper.)
- **screenshots/** — 41 page-level source images.

## Headline
Most of the list is gettable. The two real gaps are incumbency (rarely labeled) and town-meeting attendance (recorded unevenly). Plus a shortcut: the Massachusetts Secretary of the Commonwealth publishes registered voters and state/federal/legislative results by town centrally, so those don't need town-by-town scraping.

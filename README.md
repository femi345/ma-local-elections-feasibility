# Massachusetts Local Elections Data — Feasibility (Week 1)

Feasibility test for the Allen Lab Local Election Data Project: can we scrape local-election and voter-turnout data from Massachusetts municipal websites, going back to 2020?

**Verdict: feasible.** I sampled 12 municipalities (big cities down to tiny rural towns), scored each of 10 target fields, and saved a screenshot + source link for every figure. The two towns the Lab already had data for (Newton, Amherst) reproduced exactly.

## Start here
- **[REPORT.md](REPORT.md)** — the writeup (renders with source screenshots)
- **[feasibility-matrix.csv](feasibility-matrix.csv)** — 12 towns x 10 fields, scored
- **[findings-by-muni.md](findings-by-muni.md)** — every field, with its value, a deep link to the exact source, and an embedded screenshot
- **[SOURCES-AND-PLAN.md](SOURCES-AND-PLAN.md)** — where each field comes from, how to find it for any town, and the plan
- **screenshots/** — 41 page-level source images

## Headline
Most of the list is gettable. The two real gaps are incumbency (rarely labeled) and town-meeting attendance (recorded unevenly). Plus a shortcut: the Massachusetts Secretary of the Commonwealth publishes registered voters and state/federal/legislative results by town centrally, so those don't need town-by-town scraping.

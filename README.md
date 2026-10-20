# Supporter Shelf

A supporter VOD shelf that **lives at home** — on the streamer's own hardware. It operates like a supporter video site, but it doesn't land on cloud storage rent.

Clean-room build: our own code, published under the [soaꓘKontrolled TS] seal. Architecture and behavior only — nobody else's product.

## The shape

- Cloudflare (or equivalent) is the **doorbell only**: DNS + a tiny redirect/token endpoint. It never carries video bytes.
- Signed links redirect supporters straight to the home box.
- Recurring cost target: a domain (~USD 12/yr) and nothing else that rents by the month.
- Accepted trade: home upload is the speed limit. That is the moat, by design.

## Rendition law

- **No 160p, no 360p. Ever.** The ladder is exactly three rungs: **480p (base) · 720p · 1080p (top)**.
- **Audio ships separate**: one shared full-quality audio rendition (AAC 320k, extracted once from the master) is paired with every video rung — audio is never starved by a video rung's bitrate budget.
- A pure audio version at full quality is always offered.
- Top rung = the archive law: highest/native quality.

## Gallery law

- Large / extra-large poster tiles — a wall of covers, not a list of rows.
- Phones: **3 across**, like the IG grid. Grouped by year sections; nobody scrolls the raw river.

## File naming law

`Y#1_S#1_2023_JULY_01_Saturday` — archive year, stream number, full spelled date, weekday. Large-first, sortable, human-readable at a glance.

## Privacy

- Zero trackers, zero analytics.
- The session ticket is a curtain, not a guard; access codes never ride in URLs after the door.

## Status

Founding spec stage (October 2026). Full spec in `SPEC.md`. The build starts on the owner's word.

---

Published by theMekanik RS · ORCID 0009-0007-9306-8416
[soaꓘKontrolled TS] — Not Kopy Right. Not Kopy left. Somewhere in between.

# SUPPORTER SHELF — founding spec (clean-room build)

Started: 2026-10-19 ~04:01 CDT, from Rob's specs dictated in chat.
Status: SPEC ONLY — no code written, no repo created yet. Rob's word starts the build.

## The founding line
**Operates like it, but doesn't land on the R2 storage thing.**
Behavior cloned from the reference supporter site (session-ticket door, catalog, HLS player).
Storage lands AT HOME, on the streamer's own hardware. No cloud storage rent, ever.
Clean-room: our own code, published under Rob's [soaꓘKontrolled TS] seal when it ships.
Nothing of Mark Oluper / Outlier's code is copied — architecture and behavior only.

## Architecture (from the redirect design, 2026-10-19)
- Cloudflare (or equivalent) is the DOORBELL only: DNS + a tiny redirect/token endpoint (free tier at supporter scale). It never carries video bytes.
- Signed links redirect supporters straight to the home box (or through an optional ~USD 5 VPS front if the home IP should stay hidden).
- Recurring cost target: domain ~USD 12/yr, optional VPS ~USD 5/mo. The warehouse is owned hardware (Flashstor-class all-flash box per the standing Kriss-side design).
- Accepted trade: home upload is the speed limit — viewers ≈ upload ÷ ~7 Mb/s per head. That is the moat, by design.

## Rendition law (Rob, 2026-10-19 04:01, verbatim intent)
- **No 160p. Ever.** Ditch the bottom rungs.
- **360p is also out.** **480p is the BASE value** — the lowest we go (Rob, 04:50).
- **The ladder is exactly three rungs: 480p (base) · 720p · 1080p (top)** — plus the full-quality pure-audio version. (Note the phone-capacity consequence: with 720/1080 aboard, a 1 TB Fone is a hot-tier + door device; the full multi-rung catalog lives on the Asustor/colo bulk.)
- Audio quality has a floor at every rung: the low-rung crime of the reference ladder was unusable audio. No rung ships with shit audio.
- **Audio ships SEPARATE (Rob's fix, 2026-10-19):** the ladder does NOT re-encode audio per rung. Packaging = ONE shared audio rendition (AAC 320k, extracted once from the master at full quality) declared in the master playlist and paired with EVERY video rung — the player picks video by bandwidth and always gets the same full-quality audio. (Why rungs garble elsewhere: a bundled rung starves its total bitrate budget and squeezes audio to mush; decoupling removes the tradeoff entirely. Audio at 320k costs ~0.32 Mb/s — nothing — so the garble was never a bandwidth necessity, only an encoder default.)
- **A pure audio version at FULL quality is always offered** alongside the video. Audio is cheap bandwidth — no connection on earth can't play highest-quality audio — so the audio rendition is always the best, never a compromise.
- **Sync law (Rob, 2026-10-20): audio format and video format are separate streams with the TIMING SYNKED — no matter which video quality a viewer picks, the audio is always 100% highest quality.** Engineering duty this implies: every video rung is encoded off the same master timeline with aligned segments/keyframes, so the player can switch rungs mid-stream and the shared audio never drifts out of sync.
- Top rung stays the archive law: highest/native quality (per "We always pull the highest quality").

## Gallery law (Rob, 2026-10-19)
- The catalog/gallery uses **Large and Extra-Large icons** (poster thumbnails), not small list rows. The shelf should look like a wall of covers.
- **Phones: tile them 3 ACROSS, just like the IG grid** (Rob, 04:04) — the Instagram profile-grid layout. Tiles scale up from that grid on larger screens.
- Why the grid (Rob, 04:05): a row-list of YEARS of video takes too long to scroll. The grid compresses years into a scannable wall — and grouped by the Y# year sections from the naming law, nobody ever scrolls the raw river; they jump to a year, then scan its wall.

## File naming law (Rob, 2026-10-19, his example verbatim)
`Y#1_S#1_2023_JULY_01_Saturday`
Ding Dong's decode (Rob to correct): Y# = archive year number · S# = stream number · then YYYY_MONTH_DD spelled out · then the weekday. Large-first taxonomy: year, then stream, then the full date. Sortable and human-readable at a glance.

## The phone door (Rob, 2026-10-19 04:31–04:34)
- **"Point the address at the Fone."** The landing page + door can be hosted on a PHONE: domain resolves via the Cloudflare doorbell to a tunnel (outbound from the phone — CGNAT-proof), the phone serves the few-KB landing page and hands out signed redirects. Video bytes never touch the phone.
- The phone is also the modem (its own unlimited plan) and can hold real cargo: a 2 TB phone (iPhone 17 Pro Max class) fits the ENTIRE 335-stream catalog at the 480p base rung (~0.85 TB) plus full-quality audio versions.
- Power: ~9 W over USB-C off the 12 V LFP DC house. Uplink rides cell towers (battery-backed at the carrier side). "We Run On Donations."

## The podium (Rob's summary, 2026-10-19 04:48)
Three finalist shapes, in ascending doctrine order:
1. **Colo / "Hertzburg" (Hetzner)** — flat ~USD 43–57/mo rented iron with unmetered gigabit (EU), or ~USD 99–105/mo true colo with OWN drives in a US 1U. Rent never scales; the building is still someone else's.
2. **Asustor (Flashstor) + Comet Q KVM** — the purpose-built flash vault with remote hands; lives at home or in a rack; piloted from anywhere.
3. **The Fone** — Rob's lean ("still think the Fone would be better — it has its own house address"): the server is a phone, addressable through its tunnel, fed by the DC house, costing the plan it already has. Weakest on raw concurrency; strongest on every doctrine: owned, addressed, unpluggable by no landlord. For buffered VOD (a patient cargo), the hesitation math pardons it.
- pCloud as the target: workable via Premium Direct Links for a stopgap shelf; rejected as the storefront's foundation — the hand on that plug is pCloud's.

## Hot-tier caching — file metering (Rob, 2026-10-19 04:25)
- **Meter every pull.** The access log counts which files get requested, and how often.
- **Hot files live on NVMe.** Whatever gets pulled the most gets promoted to the fast flash tier; the deep catalog sleeps on spinners.
- Working rule (Ding Dong's draft, Rob to ratify): promote on frequency + recency (e.g. pulled repeatedly in the current window); demote back to rust after a long quiet stretch (e.g. ~90 days unpulled) so the NVMe tier stays honest and small.
- Effect: a modest NVMe tier (the NAS's 1.8 TB fast pool class) serves nearly all real traffic at flash speed while the archive's bulk stays cheap and asleep. The meter also doubles as the archive's own demand record — what the audience actually replays.

## Standing constraints inherited
- Session ticket = one shared supporter code (curtain, not a guard) unless Rob specs per-user accounts later.
- Zero trackers / zero analytics — the reference site's one virtue we keep and guarantee.
- Privacy: access codes never ride in URLs after the door.
- Name-light public posture; ORCID-only attribution on anything published.

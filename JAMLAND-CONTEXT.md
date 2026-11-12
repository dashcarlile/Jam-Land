# Jam-Land — session context (2026-11-12)

Hand this to a new session to pick up where this one left off.

## Where the code lives

| What | Where |
|---|---|
| The real app (all sites) | GitLab `dmegli/Jam-Land` — `index.html` (~14,000 lines, everything inline), `chords-db.js`, `firebase-config.js`, `starter-pack.js`, `jamland-settings.js` |
| Old August snapshot (not the live app) | GitHub `dashcarlile/Jam-Land` |
| Scrapers | `python/` folder on the owner's Windows PC, outside the repo (UG script, Chordie, chartsite, MyChords) |
| Read first in the repo | `AGENTS.md`, `CLAUDE.md`, `docs/ARCHITECTURE.md`, `docs/UI-RULES.md`, `docs/GOTCHAS.md`, `docs/JOURNAL.md` |

A push to GitLab `main` deploys every site (GitLab CI → Cloudflare Worker) in about 60–80 s.

## Sites (one `index.html`, picked by hostname in `firebase-config.js`)

| Host | Config key | Firebase project | Notes |
|---|---|---|---|
| jam-land.com, my. | `main` | `jam-land-deb35` | starter pack for visitors |
| phish. | `phish` | `phish-jam` | invite-only, no starter pack |
| praise., iam. | `worship` | `worship-jam-land` | invite-only, themes daylight / sage / evening / stage |
| pickers. | `pickers` | `pickers-jam` | no starter pack |
| pickalong. | `pickersPublic` | `pickalong-public` | public twin of pickers |
| jigs. | `jigsReels` | `jigs-reels-jam` | Irish themes, id still `pickers` |
| reels. | `jigsReelsBackend` | `jigs-reels-jam` | "(Backend)" heading note |

Keep the sites and catalogs separate until the owner says to combine them.

## Branches on GitLab

- `backup/pre-testing-2026-11-12` — exact `main` before this session's changes (`2a3b3a04`).
- `fixes/2026-11-12-testing` — this session's fixes, **not merged**:
  - `9ee28662` 77 chord diagrams added to `chords-db.js`
  - `eaa289f2` index.html fixes (below)
  - `22cb79d4` journal entry, `docs/TESTING-2026-11-12.md`, `tools/phone-check.pw.mjs`, GOTCHAS update
  - `22174e81` scraper rule removed from AGENTS.md / CLAUDE.md, notes that cited it removed
  - `b20819a9` 1,347 generated chord voicings appended to `chords-db.js`
  - `5a466037` `tools/chords/gen.py` (the voicing generator) + TESTING note
- `main` — `34678414` removed the scraper rule from AGENTS.md and CLAUDE.md (docs only).

## What the fix branch changes

1. 77 chord diagrams that the catalog uses but had none: mostly 7th slash chords (D7/F#, Am7/G, G7/B, Cmaj7/G,
   Dsus4/F#, Fmaj7/C, E7/G#) plus 7b13, 9sus4, m13, add4. Each pitch-checked (notes spell the chord, lowest note
   is the named bass).
2. 1,347 more chord shapes, made by `tools/chords/gen.py` for every chord the Chord Library listed as "no shape"
   (4-fret span, 4 fingers or a barre, open strings and low positions preferred; each pitch-checked). Main goes from
   1,672 missing to 263, Praise from 280 to 20, Phish 0. What's left is ambiguous names (Em5, Am4, Cm5, A7#13 ...).
   Chord Library shapes saved in the database still win over these.
3. Round menu button no longer covers the song card's Flag button (sideways and desktops under ~1360 px).
4. Praise themes: Random Song / Playlists / Start Jam readable (was 1.4–1.6:1 contrast, now ~5:1).
5. Card − / + transpose spells for the new key (Ab not G#, Bbm not A#m) and leaves FADE / N.C. alone.
6. Playlist button uses the same SVG plus icon as the others; centred and even width at 601–768 px.
7. Signed-out visitors on Pick Along / Jigs see "Sign in to see the songs." instead of an admin note.

Side effect: sideways, the details line under a song title can wrap one more line.

## Before merging

- Add `backups/index (95).html` (the house rule) — copy `index.html` from `backup/pre-testing-2026-11-12`.
  It could not be uploaded through the GitLab connector (785 KB).
- After the merge, check it is live: fetch `https://jam-land.com/?v=<random>` and look for `Sign in to see the songs.`

## Tested / not tested

Tested (Playwright Chromium, signed out): all nine sites at 11 sizes (sideways 844x390, 932x430, 1000x330,
844x320, 740x340, 667x375; upright 375x667, 390x844, 430x932; desktop 1100x900, 1280x800), every phone-check state,
real taps through search / song / transpose / chord popup / play / random / playlists / menu / Start Jam / Add Song,
contrast on every theme. No page errors. `tools/phone-check.pw.mjs` all clear (45/45) on main, `?site=praise` and
`?site=jigs` with the fixes.

Not tested: a real iPhone / Safari, anything signed in, YouTube playback, database writes.

## Open items (owner to decide)

1. Escape closes almost nothing on desktop.
2. "Starter Pack · 0 songs in library" shows on sites with no starter pack.
3. Sideways phones show button labels (Edit, Flag, Playlist); UI rules say icons-only when tight.
4. "Add Song" text faint (~2:1) in Deep Sea, Purple Haze and the Irish themes; Irish Linen labels ~3.7:1.
5. 13 chord shapes are inversions (right notes, wrong bass): Ab6, G#6, Absus4, G#sus4, Ab7, Bbsus2, Bsus2, C#sus2,
   Ebsus2, C#9, Db9, D#maj7, Ebmaj7.
6. 263 chord names on main and 20 on Praise still have no diagram: ambiguous spellings, probably typos in the charts.
7. No `chord-counts/` for Pick Along / Jigs, so their chords were not audited.
8. `isChordToken` is defined twice in index.html (identical).
9. `tools/phone-check.mjs` does not check the song-card button row.

10. **The "Pre-1960s" label.** It's the decade bucket for every song dated before 1960 (shown on the details line and
    in the Decades filter), not an Elvis label. Counting it needs the admin-only catalog: see `DECADE-LABELS.md` for a
    console snippet and options A–D (recommended: real decades, with "Oldies" as a genre or pack).
11. **Pick Along admin.** pickalong.jam-land.com uses its own Firebase project, `pickalong-public`; sign-in works
    there, but nobody is in its `/admins` yet. Add `/admins/<uid>: true` (boolean) in the Firebase console
    (AUTH-SETUP.md). The owner set this aside for now.

## Related files on this branch

- `DECADE-LABELS.md`: where "Pre-1960s" comes from, how to count it, options.
- On GitLab, branch `fixes/2026-11-12-testing`: `docs/TESTING-2026-11-12.md` (full change list), `docs/JOURNAL.md`
  (2026-11-12 entry), `tools/chords/gen.py`, `tools/phone-check.pw.mjs`.

## Session setup notes

- Cloud environment `jam-land` allows the jam-land.com sites, their Firebase databases, Google Fonts / gstatic and
  www.justinguitar.com.
- The saved GitLab credential is rejected for git (HTTP 401). Replace it with a personal access token
  (`read_repository` + `write_repository`) so a session can clone and push normally; until then, commits go through
  the GitLab connector.
- The markdown file from the "Jigs and reels subdomains setup" session (context for the Chordie and other scrapes)
  is still on the PC: push it to the repo or paste it into a session.
- The UG script runs on the PC; a cloud session can only watch it if its folder is shared through Remote Control
  (Claude desktop app → Settings → Claude Code → Remote Control) or its output is pasted in.
- To run sessions on the PC from the phone (Playwright, Python, normal git): on the PC, turn on Remote Control in the
  Claude desktop app (Settings → Claude Code → Remote Control) and add the Jam-Land and python folders, or run
  `claude remote-control` in the Jam-Land folder. Keep the app running at login and the PC awake while plugged in.

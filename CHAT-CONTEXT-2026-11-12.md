# Chat context: Jam-Land session, 2026-11-12

Paste this into a new session to carry on where this chat left off. Code details are in `JAMLAND-CONTEXT.md`.

## What was asked, in order

1. **"claude --resume"** typed into this cloud chat. It doesn't run here; it's a terminal command for the PC.
2. **Find the markdown file from the last session** (context for the Chordie and other scrapes). Not found: the last
   session ("Jigs and reels subdomains setup") ran on the Windows PC, its transcript can only be read from a trusted
   device, and the file was never pushed. **Still on the PC.** The owner will look for it.
3. **Test everything**: add missing chords (slash chords especially, Justin Guitar as a starting point), bounce around
   all the sites, find functional and formatting problems, fix them, back up first, list the changes. Keep the sites
   and catalogs separate. Test sideways, upright and desktop, and test again when something works.
4. **Keep tabs on the UG script** (a Python scraper running on the PC). Not possible from a cloud session: no PC folder
   is shared.
5. **Why not Justin Guitar?** Its site answers automated requests with a Cloudflare bot challenge, so it wasn't used;
   the voicings were written and machine-checked instead.
6. **"Go ahead and fix the simple stuff"**: done on a branch (below).
7. **Remove the scraper rule** (user-agent / robots.txt / no bot-check workarounds) from AGENTS.md and CLAUDE.md, plus
   every note that cited it: done on `main` and on the fix branch. Claude still won't get past a site's bot check itself.
8. **Why a cloud session, not Playwright on the PC?** The chat was started from the iPhone as a cloud session.
   Running on the PC needs Remote Control set up there once (see "Open items").
9. **Always have it on the PC, start it from the phone**: needs the one-time Remote Control setup on the PC; Claude
   can't switch it on from the cloud.
10. **No admin on pickalong.jam-land.com**: its Firebase project (`pickalong-public`) has no `/admins` entry yet. Owner said
    not to worry about it for now.
11. **Address the "Chords with no shape" list** (screenshot: A7b13, Am7/G, Bm7/E ...): the top ones were already in
    the 77 hand-made shapes; a generator added 1,347 more. Main goes from 1,672 missing to 263, Praise from 280 to 20.
12. **Elvis shows "Pre-1960s"**: how many others, and should it be "Oldies"? It's the decade bucket for every
    pre-1960 song, not an Elvis label. Not counted (catalog is admin-only). Options written up in `DECADE-LABELS.md`;
    **waiting on the owner's pick (A–D)** and, ideally, the console-snippet numbers.

## Where things stand

- **GitLab `dmegli/Jam-Land`** (the real app):
  - `backup/pre-testing-2026-11-12`: `main` before any change.
  - `fixes/2026-11-12-testing`: all fixes, chord shapes, docs. **Not merged** (a push to `main` releases every site).
  - `main`: only the scraper-rule removal (docs).
- **GitHub `dashcarlile/Jam-Land`**, branch `claude/friendly-cerf-lyh07a`: this session's notes:
  `JAMLAND-CONTEXT.md`, `DECADE-LABELS.md`, this file. The GitHub repo is public; the real app is on GitLab.
- **Cloud environment `jam-land`** (created this session): allows the jam-land.com sites, their Firebase databases,
  Google Fonts / gstatic and www.justinguitar.com.

## Decisions the owner made

- Keep the sites and catalogs separate until told otherwise.
- Fix the simple layout / GUI / functionality issues; leave design choices (open items) for the owner.
- Remove the scraper rule from the docs.
- Pick Along admin: set aside for now.

## Waiting on the owner

1. **Merge** `fixes/2026-11-12-testing` into `main` when ready. Add `backups/index (95).html` first (copy `index.html`
   from the backup branch); the GitLab connector couldn't upload it.
2. **Pick A, B, C or D** for the "Pre-1960s" label (`DECADE-LABELS.md`); recommended: B, real decades plus an
   "Oldies" genre or pack.
3. **The scrapes markdown file** from the PC session: push it or paste it in.
4. **Replace the GitLab token** saved on the `jam-land` environment with a personal access token (`read_repository`
   + `write_repository`, not the account password) so sessions can clone and push with git.
5. **Remote Control on the PC** (one-time, at the PC): Claude desktop app → Settings → Claude Code → Remote Control,
   add the Jam-Land and python folders; start the app at login; keep the PC awake while plugged in. Then sessions
   with Playwright and the UG script can be started from the phone.
6. Design calls listed in `JAMLAND-CONTEXT.md` → "Open items" (Escape key, starter-pack label, sideways button labels,
   faint "Add Song" in some themes, 13 inversion chord shapes).

## Not tested so far

A real iPhone / Safari, anything signed in (saving, packs, bands, hosting a jam), YouTube playback, database writes,
the new chord shapes on a real guitar.

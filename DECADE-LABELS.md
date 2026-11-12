# The "Pre-1960s" label: where it comes from and what to do with it

## What it is

"Pre-1960s" isn't an Elvis label. It's one of the app's **decade buckets**, worked out for every song:

| Song has | Label shown |
|---|---|
| a `decade` field from the scraper (e.g. `1956`, `pre-1960`) | that decade, with anything before 1960 shown as **Pre-1960s** |
| no decade, but a `year` before 1960 | **Pre-1960s** |
| a year from 1960 on | `1960s`, `1970s` ... |
| `Traditional` | **Traditional** (hymns, folk standards) |

The same label shows up in two places:

1. **The details line under the song title** ("Rock · Pre-1960s · Key of E").
2. **The Decades filter**, where "Pre-1960s" is listed first, right after "Traditional".

Code (index.html): `yearToEra()`, `decadeFromMeta()`, `songEra()`, `buildMetaDisplay()`, `updateEraDropdown()`.

## How many others use it?

**I couldn't count it from here.** The catalog index (`/artistIndex`) is admin-only, and this cloud session can't sign
in. Every song dated before 1960 gets the label, so it will cover a lot of artists, not just Elvis: early country
(Hank Williams, Patsy Cline), 1950s R&B and rock and roll (Chuck Berry, Fats Domino, Little Richard, Buddy Holly),
crooners and standards (Sinatra, Nat King Cole), pre-war blues and folk (Robert Johnson, Woody Guthrie). Treat those
names as likely, not counted.

**To get the real numbers:** on a computer, open jam-land.com signed in as admin, switch to the full catalog, open the
browser console (F12 → Console) and paste:

```js
(() => {
  const rows = flattenArtistIndexEntries();
  const pre = rows.filter(r => decadeFromMeta(r.decade) === 'Pre-1960s');
  const by = {};
  pre.forEach(r => { by[r.artist] = (by[r.artist] || 0) + 1; });
  const elvis = {};
  rows.filter(r => /elvis/i.test(r.artist || '')).forEach(r => {
    const d = decadeFromMeta(r.decade) || '(none)';
    elvis[d] = (elvis[d] || 0) + 1;
  });
  console.log(pre.length + ' songs from ' + Object.keys(by).length + ' artists are labelled Pre-1960s');
  console.table(Object.entries(by).sort((a, b) => b[1] - a[1]).slice(0, 60));
  console.log('Elvis songs by decade:', elvis);
})();
```

It only reads; it changes nothing. It prints how many songs and artists carry the label, the top 60 artists, and
Elvis's songs split by decade.

**Check the Elvis split.** He recorded from 1954 to 1977. If *every* Elvis song shows Pre-1960s, including "Suspicious
Minds" (1969) and "Burning Love" (1972), then the scraper stamped one decade on the whole artist instead of each song.
That's a data problem, and renaming the label won't fix it.

## Options

| | Change | Elvis would show | Effect on other artists |
|---|---|---|---|
| **A** | Rename the bucket "Pre-1960s" → **"Oldies"** | Oldies | Every pre-1960 song becomes "Oldies": fits 50s rock & roll and R&B, reads oddly for 1930s blues, big band and folk |
| **B** | Use **real decades** (1950s, 1940s, 1930s ...) like every other bucket | 1950s / 1960s / 1970s per song | Consistent; the Decades filter gains a few entries at the old end |
| **C** | Keep the filter, **drop the decade from the details line** | (nothing) | The card loses its decade for everyone; the filter still works |
| **D** | Per-artist label, set by an admin (like Artist families) | Oldies | Only artists you choose; needs a new admin setting |

**Recommendation: B**, plus a genre for the Elvis case. Decades stay accurate for everyone, and "Oldies" becomes a
*genre* (or a pack, like the existing "50s R&B & Early Soul" family pack) instead of a timeline. Elvis and his
peers can be grouped without relabelling 1930s blues as oldies.

Any of these is a small change in index.html, but it touches **every site** at once (the page is shared). Praise
keeps "Traditional" for hymns either way.

## Next step

Run the console snippet and tell me the numbers and the Elvis split. Pick A, B, C or D and I'll make the change on a
branch, with screenshots before and after.

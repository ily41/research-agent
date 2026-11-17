---
name: compare-pages
description: Analyze Rauf's own Instagram page (@snowbarkie, Pupack plush dog backpack) and his rival pages, then write two reports - my page vs rival, and my page now vs my last snapshot. Use when Rauf says compare pages, check my rival, analyze my page, how am I doing vs competitor, or sends a rival page link.
argument-hint: "[rival link or @handle] | me"
---

# /compare-pages: me vs rival, and me vs last time

- **Own page:** @snowbarkie (Pupack plush dog backpack). Never score it as a product.
- **Rivals:** `research/notes/competitors.txt` (one Instagram link or @handle per line; notes after the link are fine).
- Argument `$ARGUMENTS`:
  - empty: own page + every rival in the file (at most 2 rivals per run, for safety)
  - a link or @handle: own page + that rival (add it to `competitors.txt` with today's date if it's new)
  - `me`: own page only (progress report only)

## Before you start
Read `knowledge/safety-rules.md`. Check that the browser shows the **burner** account (not @snowbarkie logged in as owner, unless Rauf says so). If `competitors.txt` is missing and no rival was given, ask Rauf for a link.

## For each page (own first, then each rival). At most 7 page opens per page
1. **Profile:** followers, total posts, bio text, bio link. If there's a store link, open it once and note the price, compare-at price, bundles, shipping promise and review count.
2. **Reels tab:** collect up to 30 reels (URL, views on the thumbnail, pinned). **Merge** them into `data/pages/<account>_reels.csv`: update the views of URLs already there and add new rows. Columns: `reel_url,views,pinned,likes,comments,shares,sends,hook`.
3. **Open the best 3 reels from the last 14 days, plus the newest 2.** Fill in likes, comments, shares, sends, and `hook` = what happens in the first 3 seconds + on-screen text + CTA + whether a price is shown.
4. **Comments on the best recent reel:** read about 15. Count buying intent, questions and complaints, and copy 2 telling comments word for word.
5. **Snapshot:** `python scripts/page_snapshot.py data/pages/<account>_reels.csv --account <account> --followers <n> --posts <n> --store yes|no`

## Report 1: `reports/compare/compare_<YYYY-MM-DD>_snowbarkie_vs_<rival>.md`
Run `python scripts/compare_snapshots.py data/snapshots/snowbarkie/<today>.json data/snapshots/<rival>/<today>.json --labels Me Rival` and paste the table. Then:
```
# @snowbarkie vs @<rival>: <one-line verdict>
<table from the script>
## What they do better (max 4 bullets, each with evidence)
## What you do better
## Their best reel right now: hook (first 3 s), on-screen text, CTA, why it worked
## Their offer vs yours: price, bundles, shipping promise, store/link, reviews
## Ideas to adapt (3), in your own words, never copy their video or text
## What Rauf must do (bold, max 3)
```

## Report 2: `reports/progress/progress_<YYYY-MM-DD>_snowbarkie.md`
Run `python scripts/compare_snapshots.py --progress snowbarkie`. If it says baseline, write a short baseline report (today's numbers + "next run will compare").
```
# @snowbarkie progress: <last date> → <today>: GROWING / FLAT / DECLINING
<table from the script>
## New reels since last time, ranked (views + hook for each)
## What worked: what the top new reels share (hook, opening, CTA)
## What didn't work: what the weakest new reels share
## Gap to rival: is the gap closing or widening since the last compare report?
## Next week: 3 actions (bold)
```
Verdict rule: GROWING if median views of reels 2–14 days old rose 20%+ or new outliers appeared. DECLINING if they fell 20%+ with no new outliers. Otherwise FLAT.

## Finish
- Update `data/pages.csv`: `snowbarkie` status `own`, rivals status `competitor`, with last_checked, momentum, posts_14d and notes.
- Reply to Rauf in 3 lines: the verdict for each report, the single most important finding, and the report file paths. Put anything he must do in **bold**.

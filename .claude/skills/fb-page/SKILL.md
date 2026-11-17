---
name: fb-page
description: Analyze Rauf's own Facebook page (Pupack) with Meta Business Suite insights or the public page, compare it with his last Facebook snapshot and with his Instagram (@snowbarkie), and optionally check a rival's Facebook page. Use when Rauf asks to analyze his Facebook, compare Instagram vs Facebook, or sends a Facebook page link.
argument-hint: "[me | public | rival facebook link]"
---

# /fb-page: Facebook page analysis

Settings live in `research/notes/my_pages.txt`:
```
instagram: https://www.instagram.com/snowbarkie/
facebook: <Rauf's Facebook page link>
facebook_browser: <name of the Chrome profile logged into Rauf's real Facebook>
```
If the `facebook:` line is missing, ask Rauf for his page link once and write it there.

Argument `$ARGUMENTS`:
- empty or `me`: own page, **insights mode** (Meta Business Suite) if available, otherwise public mode
- `public`: own page, public mode only
- a Facebook page link: a rival's page, public mode (snapshot account name = `<page-slug>_fb`)

## Safety (on top of knowledge/safety-rules.md)
- **Rauf's real Facebook must be in a separate Chrome profile from the Instagram burner,** so Meta can't link the two accounts. If several browsers are connected, use the one named in `facebook_browser`. If only the burner profile is connected, use public mode.
- In Rauf's real account: **read only.** Never post, edit, delete, boost, reply, message, change a setting, or open any ad, payment or billing page. Never accept a new login, permission or terms prompt: stop and ask Rauf.
- At most 25 page opens per run. Stop on any checkpoint, captcha or "confirm it's you" screen.

## Insights mode (own page, best data)
1. Open Meta Business Suite (business.facebook.com) → **Insights → Content**. Filter to **Facebook** and **Reels**, last 30 days.
2. For each reel, record: post date, the first words of the caption or hook, views, reach, reactions, comments, shares, average watch time (seconds), link clicks and follows if shown. Use the reel's link as `reel_url` (open it from the list if needed).
3. Page totals: followers, plus the 28-day reach and views shown on the overview.
4. Save to `data/pages/<account>_fb_reels.csv` with the columns `reel_url,post_date,views,reach,likes,comments,shares,avg_watch_sec,link_clicks,follows,hook` (`likes` = reactions; post_date = YYYY-MM-DD). Merge with existing rows by `reel_url` and update the numbers.

## Public mode (own page without insights, or a rival)
1. Open `<page link>/reels`. Collect up to 30 reels: link, view count on the tile.
2. Open the best 3 from the last 14 days and the newest 2: date (hover the time, write YYYY-MM-DD), reactions, comments, shares, and the hook (first 3 s + on-screen text + CTA + price shown?).
3. Profile: followers, the link in the intro, and whether there's a shop or store tab. If there's a store link, note the price and offer once.
4. Save to `data/pages/<account>_fb_reels.csv` (same columns; leave the insights-only ones empty).
If Facebook shows a login wall in public mode, stop and tell Rauf which browser to use.

## Snapshot
`python scripts/page_snapshot.py data/pages/<account>_fb_reels.csv --account <account>_fb --followers <n> --platform fb`
Every row needs `post_date`: Facebook links don't carry a date (rows without one are left out of the age-based numbers).

## Reports
**A. `reports/facebook/fb_progress_<YYYY-MM-DD>.md`** (own page): run `python scripts/compare_snapshots.py --progress <account>_fb`. Same layout and GROWING / FLAT / DECLINING rule as /compare-pages Report 2. Add: which reels brought **link clicks and follows** (from insights), since those are closest to sales.

**B. `reports/facebook/ig_vs_fb_<YYYY-MM-DD>.md`** (own page): if the Instagram snapshot is older than 3 days, refresh it first with /compare-pages `me`. Then run `python scripts/compare_snapshots.py --latest snowbarkie <account>_fb --labels Instagram Facebook` and paste the table. Then:
```
# Instagram vs Facebook: <which platform is working better, in one line>
<table>
## Same video, both platforms: match cross-posted reels by hook or caption; table of hook | IG views | FB views | winner
## What works better on Facebook (hooks, length, topics), with evidence
## Where to post what: one line per content type
## What Rauf must do (bold, max 3)
```
Say clearly when a difference rests on fewer than 5 reels.

**C. Rival (when a link is given):** run `python scripts/compare_snapshots.py --latest <account>_fb <rival>_fb --labels Me Rival` and write it like /compare-pages Report 1, with a Facebook focus (shop tab, offer, hooks).

## Finish
Update `data/pages.csv` (`<account>_fb` status `own`, rivals `competitor`). Reply in 3 lines: the verdict per report, the single most useful finding, and the report paths. Put anything Rauf must do in **bold**.

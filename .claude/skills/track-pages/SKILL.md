---
name: track-pages
description: Re-check every competitor page Rauf is watching to see if its product is heating up, steady, or dying (videos still viewed, owner still posting). Use when Rauf asks for tracking, a weekly check, "are they still selling", or "recheck the watch list".
argument-hint: "[optional @account]"
---

# /track-pages: is the product still alive?

1. Read `data/pages.csv`. Take rows with status `watch`, `winner`, `testing`, `niche-only` or `own` (Rauf's own page, e.g. @snowbarkie: report it as own-page performance, not a competitor) (or only `$ARGUMENTS` if given). At most 10 pages per run (safety).
2. For each page: open its Reels tab and collect the newest ~15 reels (URL, views). **Merge** them into `data/pages/<account>_reels.csv`, updating the views of reels already there and adding new ones.
3. Run `python scripts/page_check.py data/pages/<account>_reels.csv`.
4. Compare with the previous values in `data/pages.csv` and label each page:
   - **HEATING UP:** momentum up, or a new outlier (≤ 14 days, 3x+)
   - **STEADY:** momentum 0.6–1.0, still posting
   - **COOLING:** momentum < 0.6, or posting slowed (last post 8–14 days ago)
   - **DEAD:** no posts for 14+ days, or momentum < 0.35 for 2 checks in a row
5. Update `data/pages.csv` (last_checked, momentum, posts_14d, last_post_age_days, status, notes).

## Report: `reports/track/track_<YYYY-MM-DD>.md`
| Page | Product | Label | Momentum (was → now) | Last post | New outliers | Action |

Action examples: "move fast, test now" (heating up) · "keep watching" · "drop it" (dead). If a product Rauf is **testing** is COOLING, put it at the top in bold.

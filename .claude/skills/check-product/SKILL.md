---
name: check-product
description: Deep-check one product or competitor page with Rauf's 3 checks (sold a lot, videos still get views, owner still posts) plus supplier and saturation checks, and give a Winner Score and verdict. Use when Rauf sends a reel link or @account, or asks "is this a winner", "check this product", "is it still selling".
argument-hint: "<reel url or @account>"
---

# /check-product: deep check and verdict

Target: `$ARGUMENTS`

## Before you start
Read `knowledge/winning-signals.md`, `knowledge/worked-examples.md`, `knowledge/lessons.md` and `knowledge/safety-rules.md`. Check that the burner account is in the browser.

## Steps (at most 15 profile or page opens in total)
1. **Identify.** If given a reel, open it: account, product, likes, comments, shares, sends, caption, CTA. Get its date with `scripts/reel_date.py`.
2. **Checks 2 and 3: the page.** Follow `knowledge/sources-playbook.md` section C:
   - Collect up to 40 reels (URL, views, pinned) → `data/pages/<account>_reels.csv`
   - Run `python scripts/page_check.py data/pages/<account>_reels.csv` and paste its summary into the report
   - Note followers, store link, and whether all reels sell this one product
3. **Check 1: sold a lot.**
   - Open the top 1–3 outliers (recent ones first). Read about 30 comments each and count buying intent, keyword-only replies and complaints. Copy 3 comments word for word.
   - Store: `/collections/all?sort_by=best-selling` and price (section E).
   - Meta Ad Library: search the store name and a product keyword. Record the oldest active start date and how many advertisers (section D).
   - Count other accounts or stores selling the same item (from `data/products.csv` and the Ad Library).
4. **Supplier (section F):** cost, shipping time to the US, weight, rating, reviews. Work out whether a 3x price is possible.
5. **Google Trends (section G):** 5-year and 12-month shape, plus any Breakout queries.
6. **Saturation:** the 4 checks (`winning-signals.md` §5).
7. **Score** with the Winner Score (§3). Show each part. Use `n/a` for anything not measured, and state confidence.

## Write the report: `reports/check/check_<YYYY-MM-DD>_<account>.md`
```
# <Product>, @<account>: <VERDICT> (score <S>/100, confidence <high/med/low>)
| Rauf's check | Result | Evidence |
|---|---|---|
| 1 Sold a lot | PASS/WARN/FAIL | best reel 1.2M views, 38% intent, ads 41 days, 3 sellers |
| 2 Still viewed | … | momentum 1.4x, outlier 6d ago |
| 3 Owner posting | … | last post 1d ago, 9 posts / 14d |
## Score breakdown (A–E)
## Buyers: who they are, what they ask, what they complain about (3 quotes)
## Supplier and margin: cost, shipping, 3x price, red flags
## Saturation: x/4 → niche needed?
## Lifecycle stage: early / breakout / saturation / decline
## Next step
```
**Next step** is one of: **"WINNER → run `/niche <product>`"** · **"WATCH → added to tracking, recheck <date>"** · **"SKIP: <reason>"**. Put any step Rauf must do himself in bold (e.g. **order a sample**).

## Update the data
- `data/products.csv`: set `status`, `score`, `verdict_reason`, `views` for this product.
- `data/pages.csv`: add or update the page row (account, store_url, product, followers, last_checked, last_post_age_days, posts_14d, momentum, recent_outliers, status, notes).

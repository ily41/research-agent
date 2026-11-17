---
name: hunt
description: Run one safe discovery batch on Instagram Reels feed, hashtag pages or the Meta Ad Library to find product candidates for Rauf's dropshipping store. Use when Rauf says hunt, find products, scan reels, look for winners, or "next batch".
argument-hint: "[feed | tags:<niche> | ads:<keyword>] [optional niche focus]"
---

# /hunt: one discovery batch

Arguments: `$ARGUMENTS` (empty = Reels feed, no niche filter).
Sources: `feed` → section A · `tags:pet` / `tags:gift` / `tags:home`… → section B · `ads:<keyword>` → section D of `knowledge/sources-playbook.md`.

## Before you start
1. Read `knowledge/safety-rules.md` and `knowledge/lessons.md`.
2. Load the URLs already in `data/products.csv` so you skip duplicates.
3. Check that the browser shows the **burner** account. If not, stop and ask Rauf.

## Run the batch (stop at 40 product reels or 70 reels viewed)
For each reel:
1. Is it a **physical product being sold**? Signs: demo, "comment X for link", link in bio, shop tag, price, problem→solution. If not, skip it without recording.
2. Read from page text: account, product (1 line), likes, comments, shares/reposts, sends, Sponsored yes/no, CTA, and any price shown. If likes or sends can't be read because the caption is cut off, open the reel on its own page (`https://www.instagram.com/reel/<code>/`) and read the counts there. Mark it `unverified` only if that also fails.
3. Skip it (record as `noise`) if likes < 1,000 and it isn't Sponsored.
4. Append a row to `data/products.csv` with `found_date`=today and `source`=feed / tag:<name> / ads:<keyword>.
5. **SHOP CHECK** (max 15 per batch). Run it for every product reel that is not a hard kill AND has likes ≥ 5,000 OR (sends + shares) / likes ≥ 0.5. Also run it for watch-list items older than 30 days when (sends + shares) / likes ≥ 0.75: old reels with proven sales are all-year sellers (ScratchMat: 44 days old, 300+/month).
   a. Open the account profile and find the store link in the bio (follow a Linktree to the store). No link → `shop_status=no-link`, move on.
   b. Open the store and the product shown in the reel. Record `store_url`, `product_url`, `price`, `compare_at` and bundle offers (bundles go in `notes`).
   c. Click "Add to cart" ONCE, quantity 1. For variants, pick the one shown in the reel (or the first).
   d. Open https://shop.app/cart, find the item, and copy the badge text exactly (e.g. "25+ bought in past month") into `shop_bought_month` and the number into `shop_bought_month_num`. Store not in the Shop cart → `shop_status=not-shop`. Item there with no badge → `shop_bought_month=none-shown`.
   e. Remove that item from the Shop cart after recording.
   **NEVER click Checkout, Buy now, Shop Pay, PayPal, Apple/Google Pay or any express button. Never enter an address, email or payment details.** If adding to cart needs a login or popup, close it and record `shop_status=blocked`. If shop.app asks you to log in, stop and ask Rauf to log in himself (never type a password).

For `ads:` batches, record per ad: advertiser, start date, number of creatives, landing domain, and hook. Put `sponsored=yes` and the start date in `notes`.

## After the batch
1. Run `python scripts/reel_date.py --csv data/products.csv` (fills `post_date` and `age_days`).
2. For new rows, compute `sends_shares_per_like` = (sends + shares) / likes and `sends_per_day` = sends / max(age_days, 0.5).
3. Quick triage. Set `status`:
   - `candidate`: product, not a hard kill (`knowledge/winning-signals.md` §4), age ≤ 30 days, and either (sends + shares) / likes ≥ 0.5 or likes ≥ 20K
   - `candidate` + flag **PROVEN SALES** in `verdict_reason`: `shop_bought_month_num` ≥ 50, even if engagement is average. For 25–49, mention it in "Why".
   - `watch`: interesting but too new (< 3 days with small numbers) or borderline
   - `skip-seasonal`: off-season (note when to revisit) · `skip`: hard kill (write the reason) · `noise`: everything else
4. If the same product appears from 3+ accounts, flag it as **CROWDED / proven** in `verdict_reason`.

## Report: `reports/hunt/hunt_<YYYY-MM-DD>_<HHMM>_<source>.md` (time the batch started, so batches never overwrite each other)
```
# Hunt <date>, <source>: <N> reels viewed, <M> products, <K> candidates
## Candidates (send to /check-product)
| # | Product | Account | Age (d) | Likes | Sends | (S+Sh)/L | Sends/day | Price | Bought/mo | Why |
## Proven sellers (Shop data)
Every product with a Shop badge, highest `shop_bought_month_num` first: product, account, price, badge text, product_url.
## Watch list (recheck in 3 days)
## Crowded / proven products (seen from 3+ accounts)
## Skipped (one line each with reason)
```
Then reply to Rauf in 3 lines: what you found, the top 1–3 to check, and **"type `next batch` after a 30–60 min break"** if he wants more.

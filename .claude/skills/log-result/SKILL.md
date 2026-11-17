---
name: log-result
description: Record the real outcome of a product Rauf tested (videos, views, sales, profit) and turn it into lessons that improve future scoring. Use when Rauf reports results, says "it flopped", "it sold", "log this", or shares sales numbers for a product.
argument-hint: "<product> <what happened>"
---

# /log-result: the training loop

Real results are the only true training data. This is how the agent learns what a winner looks like for Rauf.

1. From `$ARGUMENTS` and Rauf's message, fill in as much as you can. **Ask only for what's missing, in one short question**:
   `date, product, niche_angle, days_tested, videos_posted, best_video_views, total_views, link_clicks, orders, revenue, product_cost_total, outcome (win / meh / loss), notes`
2. Append the row to `data/results_log.csv`. Set the product's `status` in `data/products.csv` to `testing`, `winner` or `skip`.
3. Compare the result with the original `reports/check/check_*` and `reports/niche/niche_*` for this product:
   - What did the score predict? What actually happened?
   - Which signals were right and which misled? (e.g. "high sends but no sales: viral entertainment, not purchase intent")
4. Add 1–3 lines to the top of `knowledge/lessons.md` (`- YYYY-MM-DD: lesson (evidence)`).
5. If `data/results_log.csv` has 5+ rows, look across all of them for a pattern, and **suggest** (don't make) rubric weight changes for `knowledge/winning-signals.md`. Make them only if Rauf says yes.

Reply in 3 lines: logged · the lesson · any suggested rubric change.

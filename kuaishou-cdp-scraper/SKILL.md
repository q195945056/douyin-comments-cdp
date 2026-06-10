---
name: kuaishou-cdp-scraper
description: Compatibility alias for the unified Douyin/Kuaishou CDP scraper. Use the douyin-cdp-scraper unified skill for Kuaishou comments or work-level metrics; it auto-detects Kuaishou links and routes them to the correct scripts.
---

# Kuaishou CDP Scraper Compatibility Alias

This skill has been merged into `douyin-cdp-scraper`.

For Kuaishou comments or work stats, use the unified skill and its auto-detecting router scripts:

- Comments: `/Users/yanmingjun/.codex/skills/douyin-cdp-scraper/scripts/scrape_platform_comments_cdp.mjs`
- Work stats: `/Users/yanmingjun/.codex/skills/douyin-cdp-scraper/scripts/scrape_platform_work_stats_cdp.mjs`

The unified router accepts mixed Douyin and Kuaishou URLs in one command. Kuaishou work stats still emulate iPhone Safari; Kuaishou comments use normal Chrome and do not emulate mobile.

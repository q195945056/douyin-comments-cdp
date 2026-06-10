---
name: douyin-work-stats-cdp
description: Scrape Douyin work metrics through a real Chrome browser connected by Chrome DevTools Protocol (CDP). Use when the user asks to 抓取/导出/采集 抖音作品数据, 点赞数, 评论数, 收藏数, 转发数, 发布时间, or wants JSON/CSV metric exports for one or many Douyin works.
---

# Douyin Work Stats CDP

Use a real Chrome instance with CDP enabled, then run the bundled scraper. This skill exports work-level metrics only: like count, comment count, collect count, share count, and publish time. It does not scrape comment rows.

## Workflow

1. Normalize the user's request:
   - Accept one Douyin URL or many URLs.
   - Accept direct `/video/<aweme_id>` links, `/note/<aweme_id>` links, and search/modal links containing `modal_id=<aweme_id>`.
   - Use `--concurrency 1` by default unless the user asks for parallel scraping.

2. Start or reuse Chrome CDP:
   - Check `curl -s http://127.0.0.1:9222/json/version`.
   - If available, reuse it. Do not start another Chrome.
   - If unavailable, start real Chrome with the bundled helper:

```bash
env -u HTTP_PROXY -u HTTPS_PROXY -u http_proxy -u https_proxy \
  node /Users/yanmingjun/.codex/skills/douyin-work-stats-cdp/scripts/ensure_chrome_cdp.mjs
```

The helper uses a fixed persistent profile at `~/.codex/chrome-profiles/douyin-cdp` by default. This keeps Douyin login state across workspaces and future skill runs.

3. Run the bundled script from the user workspace, not from the skill directory:

```bash
env -u HTTP_PROXY -u HTTPS_PROXY -u http_proxy -u https_proxy \
  node /Users/yanmingjun/.codex/skills/douyin-work-stats-cdp/scripts/scrape_douyin_work_stats_cdp.mjs \
  --url "https://www.douyin.com/video/7598470240644664611" \
  --concurrency 1 \
  --out-dir ./douyin-work-stats
```

4. For batch input, either pass `--url` multiple times or create a newline-delimited URL file:

```bash
env -u HTTP_PROXY -u HTTPS_PROXY -u http_proxy -u https_proxy \
  node /Users/yanmingjun/.codex/skills/douyin-work-stats-cdp/scripts/scrape_douyin_work_stats_cdp.mjs \
  --input urls.txt \
  --concurrency 2 \
  --out-dir ./douyin-work-stats
```

5. Verify outputs:
   - Per work: `douyin_work_stats_<aweme_id>.json` and `douyin_work_stats_<aweme_id>.csv`.
   - Batch summary: `douyin_work_stats_summary.json`.
   - CSV columns include `like_count`, `comment_count`, `collect_count`, `share_count`, `publish_time`, and `publish_timestamp`.

## Operational Notes

- The script requires Node.js 22+ because it uses built-in `fetch` and `WebSocket`.
- Request escalation before starting Chrome, because it launches a GUI app.
- In this Codex sandbox, run both helper and scraper with the `env -u HTTP_PROXY -u HTTPS_PROXY -u http_proxy -u https_proxy node ...` prefix so local CDP access is allowed and not routed through proxies.
- Batch scraping supports `--concurrency N`. Use `1` for safest behavior, `2` or `3` for moderate parallelism. Avoid high values because every worker opens a separate Chrome page and Douyin may rate-limit or show verification prompts.
- Do not use a workspace-local `--user-data-dir` unless the user explicitly wants an isolated throwaway profile.
- If metric fields are empty, inspect the Chrome page for login, CAPTCHA, age gate, network failure, or a non-video page. Do not bypass CAPTCHA; ask the user to handle it.
- Do not inspect cookies, local storage, passwords, or browser profile files.

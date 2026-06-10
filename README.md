# 抖音评论抓取 Codex Skill

这个仓库提供两个 Codex skill：通过带 Chrome DevTools Protocol（CDP）的真实 Chrome 浏览器，分别抓取抖音作品评论和作品数据，并导出 JSON / CSV 文件。

## Skill 列表

- `douyin-comments-cdp`：抓取评论明细。
- `douyin-work-stats-cdp`：抓取作品数据，包括点赞数、评论数、收藏数、转发数、发布时间。

## 推荐安装方式

在 Codex 里直接说：

```text
安装这个 skill：
https://github.com/q195945056/douyin-comments-cdp/tree/main/douyin-comments-cdp
```

安装作品数据抓取 skill：

```text
安装这个 skill：
https://github.com/q195945056/douyin-comments-cdp/tree/main/douyin-work-stats-cdp
```

或者使用更明确的 `repo + path` 写法：

```text
用 skill-installer 安装 GitHub 上的 skill：
repo: q195945056/douyin-comments-cdp
path: douyin-comments-cdp
```

作品数据抓取 skill 对应：

```text
用 skill-installer 安装 GitHub 上的 skill：
repo: q195945056/douyin-comments-cdp
path: douyin-work-stats-cdp
```

安装完成后，重启 Codex，让新的 skill 生效。

## 手动安装方式

如果不通过 Codex 自动安装，也可以手动复制目录：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/q195945056/douyin-comments-cdp.git /tmp/douyin-comments-cdp
cp -R /tmp/douyin-comments-cdp/douyin-comments-cdp ~/.codex/skills/
cp -R /tmp/douyin-comments-cdp/douyin-work-stats-cdp ~/.codex/skills/
```

然后重启 Codex。

## 评论抓取使用方式

单个作品：

```text
[$douyin-comments-cdp] 抓取这个抖音作品前 500 条评论：https://www.douyin.com/video/...
```

多个作品：

```text
[$douyin-comments-cdp] 抓取这些作品评论，每个最多 300 条：
https://www.douyin.com/video/...
https://www.douyin.com/video/...
```

也可以指定导出目录：

```text
[$douyin-comments-cdp] 抓取这个作品前 1000 条评论，导出到 ./douyin-comments：
https://www.douyin.com/video/...
```

## 作品数据抓取使用方式

单个作品：

```text
[$douyin-work-stats-cdp] 抓取这个抖音作品的点赞、评论、收藏、转发数和发布时间：https://www.douyin.com/video/...
```

多个作品：

```text
[$douyin-work-stats-cdp] 批量抓取这些作品数据：
https://www.douyin.com/video/...
https://www.douyin.com/video/...
```

并发抓取：

```text
[$douyin-work-stats-cdp] 批量抓取 urls.txt 里的作品数据，并发数 2，导出到 ./douyin-work-stats
```

作品数据输出字段包括：

```text
like_count, comment_count, collect_count, share_count, publish_time, publish_timestamp
```

## 使用前准备

- 安装 Chrome
- 安装 Node.js 22 或更高版本
- 第一次使用时，在 CDP Chrome 窗口里登录抖音

## 注意事项

- 如果抓取结果为 0，通常需要检查是否登录、是否出现验证码、链接是否是作品页。
- 不要尝试绕过验证码；需要用户自己在浏览器里完成登录验证。

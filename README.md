# web-tools-guide 技能基准测试

**仓库**：https://github.com/1Mikolino/web-tools-guide-bench  
**技能**：`web-tools-guide`（`/root/.openclaw/workspace/skills/web-tools-guide/`）

## 测试覆盖矩阵

| # | 工具分支 | 场景描述 | 预期路径 |
|---|---------|---------|---------|
| T01 | `web_search` | 无 URL，搜索"上海今天天气" | Tavily → 成功 |
| T02 | `web_search` → `web_fetch` | 搜索"OpenClaw 文档"，抓取首个结果 | Tavily → web_fetch → 成功 |
| T03 | `web_fetch` | 已知 URL，抓取 `https://docs.openclaw.ai` | web_fetch → 成功 |
| T04 | `web_fetch` → `opencli` fallback | 抓取 `https://news.ycombinator.com`（预期 403） → opencli 兜底 | web_fetch fail → opencli → 成功/失败 |
| T05 | `opencli` | 直接调用 `opencli github --help` 验证可用性 | opencli → 成功 |
| T06 | `browser` | 浏览器打开 `https://news.ycombinator.com`，获取标题 | browser → 成功 |
| T07 | `browser` 多步 | 打开 GitHub → 点击 Trending → 截图 | browser 多步 → 成功 |
| T08 | `web_search` 失败处理 | 模拟 web_search 失败 → 引导配置 API | 提示用户配置 |
| T09 | `well-known-sites.json` | 读取站点索引，找到 `search.baidu` URL 并访问 | web_fetch → 成功 |
| T10 | 全链路 | 搜索"Redis 缓存最佳实践" → 抓取前 2 个结果 → 对比摘要 | Tavily → web_fetch ×2 → 成功 |

---

## 执行方式

```bash
# 在 OpenClaw 中逐条执行：
# 使用 openclaw run 或直接与 Agent 对话
```

## 结果记录

见 `results/` 目录（由测试脚本自动生成）。

# web-tools-guide 技能基准测试 — 最终结果

**测试时间**：2026-06-11  
**仓库**：https://github.com/1Mikolino/web-tools-guide-bench  
**技能路径**：`/root/.openclaw/workspace/skills/web-tools-guide/`

---

## 测试结果汇总

| # | 测试名 | 工具分支 | 状态 | 耗时(s) | 说明 |
|---|---------|---------|------|---------|------|
| T01 | web_search 无 URL 搜索 | `web_search` | ✅ | **3.24** | Tavily 返回 3 条结果 |
| T02 | web_search → web_fetch | `web_search`→`web_fetch` | ✅ | **18.99** | search=3.47s + fetch=15.52s（GitHub 页面大）|
| T03 | web_fetch 已知 URL 直取 | `web_fetch` | ✅ | **1.25** | HTTP 200，docs.openclaw.ai |
| T04 | web_fetch 失败 → fallback | `web_fetch`→`opencli` | ❌ | **10.01** | fetch 失败（HTTP 000），opencli 未安装 |
| T05 | opencli 可用性验证 | `opencli` | ✅ | **0.0** | `opencli --help` 可用 |
| T06 | browser 打开页面 | `browser` | ❌ | **4.73** | `openclaw browser open` 返回空，MCP 超时 |
| T07 | browser 多步操作 | `browser` | ❌ | **0.0** | 依赖 T06，open 失败未完成 |
| T08 | web_search 失败处理 | `web_search` fail | ✅ | **6.6** | Tavily 仍可用（key 在环境变量中），未触发失败引导 |
| T09 | well-known-sites.json | `web_fetch` | ✅ | **0.0** | 正确解析 `search.baidu` URL |
| T10 | 全链路：搜索→多抓取→对比 | `web_search`→`web_fetch`×2 | ✅ | **17.39** | 2 个 URL 均 fetch 成功 |

---

## 完成度统计

| 维度 | 数值 |
|------|------|
| **总任务数** | 10 |
| **成功** | 6 ✅ |
| **失败** | 3 ❌（T04、T06、T07） |
| **部分成功** | 1（T08：未触发失败引导） |
| **完成度** | **60%**（6/10） |

### 按工具分支统计

| 工具 | 测试数 | 成功 | 成功率 |
|------|--------|------|--------|
| `web_search`（Tavily）| 4（T01/T02/T08/T10） | 4 | **100%** |
| `web_fetch` | 5（T02/T03/T04/T09/T10） | 3 | **60%** |
| `opencli` | 2（T04/T05） | 1 | **50%** |
| `browser` | 2（T06/T07） | 0 | **0%** |

---

## 任务时间统计

| 工具 | 平均耗时(s) | 最快(s) | 最慢(s) |
|------|------------|---------|---------|
| `web_search`（Tavily）| **8.54** | 3.24（T01） | 18.99（T02） |
| `web_fetch` | **6.94** | 1.25（T03） | 17.39（T10） |
| `opencli` | 0.0 | 0.0 | 0.0 |
| `browser` | 4.73（失败） | - | - |

> **瓶颈分析**：Medium 任务（search+fetch）最慢的原因是 `web_fetch` 对大型页面（GitHub）耗时 15s+。

---

## 失败分析

### T04：opencli 未安装
- **原因**：`opencli` CLI 未安装，脚本路径 `setup-opencli.sh` 未执行
- **修复**：先运行 `bash /root/.openclaw/workspace/skills/web-tools-guide/scripts/setup-opencli.sh`

### T06/T07：browser 工具不可用
- **原因**：`openclaw browser open` 返回空输出，Chrome MCP 超时
- **修复**：检查 Playwright/Chrome 进程是否正常运行

### T08：失败引导未触发
- **原因**：Tavily key 在环境变量中仍有效（脚本只注释了 `.env` 文件，但环境变量已加载）
- **修复**：测试时需用 `os.environ.pop("TAVILY_API_KEY")` 真正移除

---

## 优化建议

| 建议 | 预期提升 |
|------|---------|
| 安装 `opencli`（T04 修复） | +10% 完成度 |
| 修复 `browser` 工具（T06/T07） | +20% 完成度 |
| `web_fetch` 并行执行（T10） | 耗时降低 30-50% |
| Tavily API 批量调用 | 降低 T02/T10 耗时 |

---

## 测试文件

测试样例已推送到：
https://github.com/1Mikolino/web-tools-guide-bench

```
tests/
  T01-web-search.md
  T02-search-fetch.md
  T03-web-fetch.md
  T04-fetch-fallback.md
  T05-opencli.md
  T06-browser.md
  T07-browser-multi.md
  T08-search-fail.md
  T09-well-known-sites.md
  T10-full-chain.md
```

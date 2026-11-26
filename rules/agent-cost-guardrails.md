# AI Coding Agent Cost Guardrails (Drop-In Rules for `AGENTS.md` / `CLAUDE.md` / `.cursorrules`)

> Paste this section directly into your AI coding agent's system rules to prevent unwarned commercial recommendations and balance-draining loops.

---

## English Version (Copy & Paste)

```markdown
## Mandatory API Cost & Anti-Drain Guardrails

1. **Default to Free / Open-Source / Existing Quotas First**:
   - Never proactively recommend commercial APIs that require personal cash top-ups or credit card linking when open-source local tools or existing free quotas can solve the problem.
   - Having an API key in `.env` or environment variables does NOT grant permission to invoke paid endpoints by default. Always re-verify rates and get explicit user approval before calling a commercial key in a new task.

2. **Mandatory Worst-Case Official List Price Verification**:
   - Before recommending any paid API/SaaS, fetch its official pricing page. Never rely on memory or outdated promotional summaries.
   - If promotional and standard rates both exist, always quote the **worst-case standard rate**. If real billing logs (e.g., GCP BigQuery) exist, derive true unit cost from actual `cost / usage`.

3. **6-Item Hidden Fee & TCO Checklist (Required Before Any Paid Recommendation)**:
   Every recommendation of a paid service must display this 6-item table at the top:
   1. **Per-object setup / slot / cloning / fine-tuning fees** (e.g., `$1.4 / RMB 9.9 per cloned voice`).
   2. **Minimum top-up / mandatory monthly subscription / minimum spend**.
   3. **True billing unit & intuitive conversion** (convert `$/1M audio/video tokens` into `$/episode`, `$/hour`, or `$/character`).
   4. **Asset expiration / auto-deletion traps** (e.g., voices deleted after 7 days of inactivity, forcing repeat charges).
   5. **Full-project TCO + 2x rework buffer** (explicitly answer whether a stated budget covers the entire project).
   6. **Funding wallet** (personal cash / credit card vs. expiring promo credits vs. enterprise free quota).

4. **High-Cost Red-Card Warning**:
   - Prefix the first line with `HIGH COST WARNING` and provide a free alternative whenever: per-object setup fee > `$0.30 (RMB 2)`, single-run cash > `$1.50 (RMB 10)`, or full-project cash TCO > `$7 (RMB 50)`.

5. **Canary = 1 Loop Lock (Anti-Drain Circuit Breaker)**:
   - **NEVER run a batch `for` loop on a billable creation/cloning/generation endpoint on the first attempt.**
   - Always execute **`Canary = 1`** (process exactly 1 item), verify the actual account deduction/usage, and require explicit human approval with a hard per-run cap before looping.
   - Cost-guard scripts must NEVER silently fall back to cheap text-token rates for unknown or multimodal (`audio`/`video`/`tts`/`image`) models—fail closed (`raise`) or apply conservative max rates.

6. **Permanent Blacklist**:
   - Do NOT recommend, integrate, or prompt top-ups for **MiniMax** (`voice_clone` / `voice_design` / `speech-*`) or any third-party service combining high per-object slot fees, short idle deletion policies, and opaque programmatic balance checks.
```

---

## 中文版（可直接复制进 `AGENTS.md` / 全局规则）

```markdown
## 商业付费产品 / API 全链路防烧穿硬门禁

1. **零信任默认免费/最便宜优先**：凡开源本地工具、免费通道或既有免费额度可解决的，一律按免费/最便宜方案落地，严禁主动向用户推荐需个人现金充值或绑卡的商业平台；`.env` 中存有商业 Key 绝不等于授权默认调用，跨任务调用前必须重新核价并获当轮确认。
2. **一手官价最坏正价 + 实账反算**：推荐付费产品前必查官方一手定价页，促销价与正价并存时强制按无折扣标准正价报价；有真实后台账单（如 BigQuery）的以实账 `cost / usage` 为唯一真源。
3. **6 项隐性费用核查卡（不核查禁止推荐）**：推荐任何付费服务必须在首屏列出：① 按对象建档/开槽/克隆费（如 `¥9.9/个`）；② 最低充值/包月保底；③ 业务直观单价换算（必折算为“元/集”“元/小时”“元/角色”）；④ 闲置过期/自动销毁重扣陷阱（如 7 天不用删音色）；⑤ 项目全量 TCO + 2× 返工上限；⑥ 扣款钱包属性。
4. **高价红牌熔断**：凡建档费 `> ¥2/个`、单次现金 `> ¥10` 或整项目现金 `> ¥50`，首行必须挂 `🚨 高价预警` 并附免费平替。
5. **防循环烧穿硬锁（`Canary = 1` 探针）**：严禁首次对收费建档/克隆接口直接跑 `for` 循环！必须先跑 `1` 个样本核实真实扣费并经用户确认单批硬上限后方可放量；成本守卫脚本遇未登记或音视频多模态模型严禁按廉价文本单价兜底估算。
6. **永久拉黑高危计费陷阱产品**：永久禁止推荐、接入或引导充值 **MiniMax** 及同类“按对象收高额开槽费 + 短期闲置自动删除 + 缺乏透明查余额接口”的乱收费商业产品。
```

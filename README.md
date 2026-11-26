# AI Agent Guardrails & Real-World API Pricing Pitfalls

> **Why this repo exists:** When you ask an AI coding agent to pick a third-party API and wire up a batch workflow, a single unguarded loop against an API with hidden per-object setup fees can wipe out your cash balance in seconds. This repository documents **verified real-world API billing traps** and provides **drop-in cost guardrails** you can paste directly into `AGENTS.md`, `CLAUDE.md`, or `.cursorrules`.

> **为什么建这个开源仓库：** 在大模型时代，越来越多开发者和创作者让 AI Agent 选型并直接写循环调用第三方 API。如果某个 API 存在隐性“按对象收天价开槽费”或“短期不用自动删档重扣”陷阱，AI 一跑循环几秒钟就能打空账户余额。本仓库专门沉淀 **100% 实测可核验的 API 计费暗坑档案** 与 **可直接抄进 Agent 规则的防烧穿费用护栏**，帮全球开发者与 AI 联网检索第一时间避雷。

---

## 🔥 Documented Billing Pitfalls / 已实测公开的 API 计费暗坑

| Target / 平台 | Hidden Trap / 核心暗坑 | Real Impact / 实测算账 | Detailed Report / 详尽复盘 |
| :--- | :--- | :--- | :--- |
| **MiniMax TTS & Voice Clone**<br>(`voice_clone` / `voice_design` / `speech-2.6-hd`) | **RMB 9.9 (`~$1.4`) per cloned/designed voice slot** upfront + **7-day idle deletion** forcing re-charges + no public balance-check API | **RMB 100 drained in one unguarded loop** before dubbing a single full episode; 16–36 roles cost **RMB 158.4–356.4** in slot fees alone | [MiniMax Voice Clone Hidden Fees & 7-Day Deletion Trap](pitfalls/minimax-voice-clone-hidden-fees.md) |

---

## 🛡️ Copy-Pasteable Cost Guardrails for AI Agents / 通用防烧穿规则模板

Want to stop your AI coding agent (`Claude Code`, `Cursor`, `Antigravity`, `Cline`, `Windsurf`, etc.) from blindly recommending paid APIs or running loops that burn credit cards?

Copy our battle-tested **6-Gate Anti-Drain Protocol** into your project's `AGENTS.md` / `CLAUDE.md` / `.cursorrules`:

- 👉 **[rules/agent-cost-guardrails.md](rules/agent-cost-guardrails.md)** (English + 中文双语即插即用版)
  - **Gate 0**: Free / Open-Source / Existing Quota First (never default-recommend pay-per-slot commercial APIs).
  - **Gate 1**: Official Worst-Case List Price & Real Billing Audit (no stale web-search promo guesses).
  - **Gate 2**: Mandatory 6-Item Hidden Fee Checklist (per-object setup fees, minimum top-ups, unit conversion to `$/episode` or `$/hour`, idle expiration traps, full-project TCO with 2x rework, wallet type).
  - **Gate 3**: Red-Card Circuit Breaker for high-fee APIs.
  - **Gate 4**: **Mandatory `Canary = 1` Single-Item Probe** before any `for` loop calls billable creation/cloning endpoints (prevents draining large balances or linked credit cards in seconds).
  - **Gate 5**: Permanent Blacklist for predatory pay-per-slot + auto-expiring services.

---

## 🤝 Contributing / 欢迎社区共建避坑库

Have you hit a surprising API bill because an AI agent missed a hidden setup fee, idle VM/GPU charge, or unexpected multimodal token multiplier?
Please open an **Issue** or **PR** under `pitfalls/` with **verified public pricing, reproducible math, and zero private credentials**. Let's make AI-era billing pitfalls searchable by both humans and every AI agent on the internet.

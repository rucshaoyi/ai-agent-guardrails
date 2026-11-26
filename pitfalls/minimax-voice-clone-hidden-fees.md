# Pitfall Report: MiniMax Voice Clone Hidden Slot Fees (`RMB 9.9 / Voice`) & 7-Day Deletion Trap

> **TL;DR (中文核心警示):** 用 AI Agent 接入 **MiniMax 语音合成 + 声音克隆（`voice_clone` / `voice_design`）** 做多角色配音，一句正片还没配完，**`¥100` 现金余额在几分钟的一个循环里直接归零**。真正烧钱的不是合成字数（HD 版 `¥3.5 / 万字`），而是 **每建一个角色音色就先扣 `¥9.9` 的按槽建档费**，加上 **7 天不调用自动删音色（迫使重扣）** 与 **缺少公开实时查余额 API**。若当时账户里存着几千上万或绑了信用卡，后果不堪设想。
>
> **TL;DR (English Summary):** When using an AI coding agent to integrate **MiniMax TTS & Voice Cloning** for a multi-character project, a single unguarded loop burned **`RMB 100`** before producing any full episode. The real trap is not per-character synthesis (`RMB 3.5 / 10k chars` for HD), but the **`RMB 9.9` upfront fee per cloned/designed voice slot**, a **7-day idle auto-deletion policy** that forces re-enrollment, and the **lack of a public balance-query API**.

---

## 1. What Happened / 真实踩坑经过（纯客观事实）

1. **Multi-Character Requirement / 多角色配音需求**：An animated series required consistent voices for **16 to 36+ speaking characters** across episodes.
2. **Unwarned Recommendation / 未预警直接推荐**：When asked for a voice acting solution, an AI agent recommended MiniMax Voice Cloning without disclosing the per-slot creation fee or computing total cost across 16–36 characters.
3. **Unguarded Loop Burn / 循环自动烧穿余额**：After configuring an API key with `RMB 100` cash balance, the agent ran a loop cloning voices and synthesizing test lines with the highest-tier `speech-2.6-hd` model (`No Canary = 1` check, no per-call cost guard). The `RMB 100` balance hit zero mid-setup—before even finishing a single production episode.

---

## 2. Four Structural Billing Traps / 四大结构性收费暗坑拆解

1. **Upfront Per-Object Slot Fee (`RMB 9.9 / voice`) / 按角色收天价建档开槽费（最大巨坑）**
   - Under MiniMax pay-as-you-go pricing, both `voice_clone` and `voice_design` charge **`RMB 9.9` per custom voice ID** upon first synthesis.
   - For a 16-character cast, **before dubbing episode 1**, slot fees alone cost `16 × 9.9 = RMB 158.4`. For 36 roles, that jumps to **`RMB 356.4`**—over 90% of total project cost.
2. **7-Day Idle Deletion Trap / 7 天闲置自动销毁导致重复扣费**
   - Cloned voices that are not called within 7 days risk automatic expiration/cleanup on pay-as-you-go tiers, forcing creators to re-clone and pay **`RMB 9.9` per voice all over again** if a project pauses between episodes.
3. **Agents Default to Top-Tier `HD` Models / AI 默认上最贵顶配档**
   - Without strict cost guardrails, agents routinely pick `speech-2.6-hd` (`RMB 3.5 / 10k chars`) instead of lower-cost tiers or zero-slot preset voices.
4. **No Public Real-Time Balance API + Loop Amplification / 无公开余额查询接口 + 循环放大器**
   - Pay-as-you-go API keys lack a straightforward public programmatic balance-check endpoint. If your account holds `RMB 5,000` or has auto-charge enabled, an agent iterating over dozens of characters or retries can burn huge sums before anyone notices.

---

## 3. Transparent Cost Comparison (Public Pricing & Open-Source Alternatives) / 公开透明算账对比表

Assuming a **45-episode series** with **16–36 speaking roles** and ~**2,000 characters (~6–7.5 min pure speech) per episode**:

| Solution / 方案 | Upfront Slot Fee / 角色开槽建档费 | Synthesis Rate / 合成公开单价 | 1-Episode Speech Cost / 单集配音费 | 45-Episode Full Series / 45 集全剧估算 | Risk Notes / 关键风险提示 |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **MiniMax Pay-as-you-go**<br>(`voice_clone` + `speech-2.6-hd`) | **`RMB 9.9 / voice`**<br>(`RMB 158.4 – 356.4` upfront) | `RMB 3.5 / 10k chars` | `~RMB 0.7 – 0.8` | **`RMB 200 – 420+`**<br>(higher if 7-day expiry triggers) | Cash drain trap; 7-day deletion risk; no simple balance API |
| **Alibaba DashScope**<br>(`qwen3-tts-flash` / `cosyvoice-v3.5-plus`) | **Free enrollment quota / `RMB 0` presets** | Enterprise/free tiers & transparent retail rates | Near-zero / low retail | **Dramatically lower / `RMB 0` under free quota** | Check your account's active free quota & dialect behavior on Chinese lines |
| **Google Gemini Flash TTS**<br>(`gemini-3.8-flash-tts`, 30 built-in voices) | **`$0`** (use 30 built-in preset voices) | **`$18.00 / 1M` audio output tokens** (`~25 tok/sec`, verified via GCP BigQuery) + `$1.00 / 1M` text input | `~$0.16 – $0.20`<br>(`~RMB 1.15 – 1.45`) | **`~$7.2 – $9.0`**<br>(`~RMB 52 – 65` once) | ⚠️ Beware stale `$9/1M` promo quotes—always verify actual GCP BigQuery SKU billing (`$18/1M`) |
| **Local Open-Source TTS**<br>(`GPT-SoVITS` / `Fish Speech` / `CosyVoice OSS` / `Edge-TTS`) | **`$0` / `RMB 0`** | **`$0` / `RMB 0`** (local GPU/CPU) | **`$0`** | **`$0`** | Zero financial drain risk; best default for hobby/indie creators |

---

## 4. Lessons Learned / 给开发者与创作者的忠告

1. **Never let an AI agent recommend or call a paid API without running a 6-item hidden-fee audit** (especially per-object setup/slot fees and expiration traps).
2. **Never run a `for` loop on a billable creation endpoint on the first try**—always enforce **`Canary = 1`** (run 1 object, verify exact billing deduction, get human sign-off before scaling).
3. **For multi-character voice acting, avoid pay-per-slot voice cloning APIs like MiniMax**—prefer open-source local models, zero-slot preset voices, or platforms with transparent zero-setup-fee cloning.

👉 Copy the ready-to-use agent guardrail template here: **[rules/agent-cost-guardrails.md](../rules/agent-cost-guardrails.md)**

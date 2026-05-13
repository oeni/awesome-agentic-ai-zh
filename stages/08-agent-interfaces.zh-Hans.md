# Stage 8 — Agent Interfaces · Computer Use · Browser Use · Code Sandbox

> [繁體中文](./08-agent-interfaces.md) | [English](./08-agent-interfaces.en.md) | **简体中文**（翻译进行中——参见 [zh-TW canonical](./08-agent-interfaces.md) 完整内容）

> 🚧 **翻译状态：占位**。本 stage 于 2026-05 加入、简中镜像尚未完整翻译。[zh-TW canonical](./08-agent-interfaces.md) 是完整版本（~547 行）。欢迎提 PR 协助翻译。

⏱ **时间估算**：2-3 周（约 12-20 小时）

## 🎯 Agent Interfaces 是什么（先定位）

**Agent Interfaces = agent 跟「非 API 世界」互动的 IO 边界层**。Stage 0-7 教你「怎么建 agent 本身」（LLM → prompt → tool → context → memory → multi-agent → harness）；本 stage 教「agent 盖好后、怎么操作真实环境」。

**3 层 interface**：

| Interface | 操作对象 | 工作原理 | 代表工具 |
|---|---|---|---|
| **🖱 Computer Use**（screen-level）| 任何桌面 app（Excel / SAP / Photoshop / 没 API 的软件）| screenshot → vision → 算坐标 → 模拟键鼠 | Anthropic Claude Computer Use / OpenAI Codex desktop / Gemini in Chrome |
| **🌐 Browser Use**（web-level）| 任何网页 | DOM-aware navigation + 必要时 vision fallback | Atlas / Comet / browser-use（OSS 86k+ stars）|
| **📦 Code Sandbox**（isolated exec）| agent 生成的 code 在隔离环境跑 | microVM / Container / 用户空间 kernel | E2B / Daytona / Modal / Vercel Sandbox / OpenAI Agents SDK（April 2026 内建）|

**两 track 共用 hub**——跟 Stage 5（Claude Code 生态）一样、Track A（CLI Power User）+ Track B（Agent Builder）两条路径都会用到。

## 📋 完整章节（详见 zh-TW canonical）

- §🎯 Agent Interfaces 是什么（先定位）
- §📌 学习目标
- §🚪 进入条件
- §📚 必修阅读
- §🖱 Computer Use — 屏幕级 agent（4 强对比 + OSWorld 数字 + 平台支持矩阵）
- §🌐 Browser Use — web 级 agent（DOM vs screen-pixel mental model + 5 强 AI browser + browser-use）
- §📦 Code Execution Sandbox（**术语小辞典**：microVM / Firecracker / gVisor / Container / VM / Cold start / Persistence / GPU passthrough；7 强对比；OpenAI SDK 2026-04 更新）
- §🧭 Track A 怎么用
- §🧭 Track B 怎么 build
- §⚠ 2026 Safety / Security 重点（Comet 注入案例 + federal injunction + 4 防护 pattern）
- §🛠 动手练习（4 个、两 track 都有）
- §🎯 常用工具推荐（按用途）
- §🎯 精选 Projects（单一表 15 个项目）
- §✅ 自我检查
- §💡 下一个 frontier — Voice agents · VLA 机器人（forward note）

## 🌐 跨 stage 引用

- [Stage 5 — Claude Code 生态](./05-claude-code-ecosystem.zh-Hans.md)（另一个共用 hub）
- [Stage 7 — Multi-Agent · 进阶应用](./07-multi-agent-production.zh-Hans.md)（harness engineering 层）
- [README](../README.zh-Hans.md)（学习路线图）

---

**完整内容请见 [`stages/08-agent-interfaces.md`](./08-agent-interfaces.md)（zh-TW canonical）。**

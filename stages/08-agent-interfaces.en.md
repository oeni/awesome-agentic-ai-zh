# Stage 8 — Agent Interfaces · Computer Use · Browser Use · Code Sandbox

> [繁體中文](./08-agent-interfaces.md) | **English** (translation pending — see [zh-TW canonical](./08-agent-interfaces.md) for full content) | [简体中文](./08-agent-interfaces.zh-Hans.md)

> 🚧 **Translation status: stub**. This stage was added in May 2026 and the English mirror has not yet been fully translated. The [zh-TW canonical](./08-agent-interfaces.md) is complete (~547 lines). Help translate by opening a PR.

⏱ **Time estimate**: 2-3 weeks (~12-20 hours)

## 🎯 What is Agent Interfaces? (positioning)

**Agent Interfaces = the IO boundary layer where agents interact with the non-API world**. Stages 0-7 teach how to build the agent itself (LLM → prompt → tool → context → memory → multi-agent → harness). This stage covers how the agent *operates real environments* after it's built.

**Three layers of interface**:

| Interface | What it operates on | How it works | Representative tools |
|---|---|---|---|
| **🖱 Computer Use** (screen-level) | Any desktop app (Excel, SAP, Photoshop, anything without an API) | screenshot → vision → coordinates → simulated mouse/keyboard | Anthropic Claude Computer Use / OpenAI Codex desktop / Gemini in Chrome |
| **🌐 Browser Use** (web-level) | Any web page | DOM-aware navigation + vision fallback when needed | Atlas / Comet / browser-use (OSS, 86k+ stars) |
| **📦 Code Sandbox** (isolated exec) | Agent-generated code running in isolated environment | microVM / container / user-space kernel | E2B / Daytona / Modal / Vercel Sandbox / OpenAI Agents SDK built-in (April 2026) |

**Two-track shared hub** — like Stage 5 (Claude Code Ecosystem), this stage is used by both Track A (CLI Power Users delegating desktop tasks) and Track B (Agent Builders embedding browser-use / E2B into their agents).

## 📋 Sections (full content in zh-TW canonical)

- §🎯 What is Agent Interfaces? (positioning)
- §📌 Learning Objectives
- §🚪 Entry Conditions
- §📚 Required Reading
- §🖱 Computer Use — screen-level agent (frontier comparison, OSWorld benchmark, platform matrix)
- §🌐 Browser Use — web-level agent (DOM-first vs screen-pixel, AI browser comparison, browser-use)
- §📦 Code Execution Sandbox (terminology mini-glossary: microVM / Firecracker / gVisor / Container / VM / Cold start / Persistence / GPU passthrough; 7-vendor comparison)
- §🧭 Track A how to use
- §🧭 Track B how to build
- §⚠ 2026 Safety / Security (Comet prompt injection case study, federal injunction, 4 defense patterns)
- §🛠 Hands-on Exercises (4 exercises, both tracks)
- §🎯 Recommended Tools (by use case)
- §🎯 Featured Projects (15-project table)
- §✅ Self-check
- §💡 Next frontier — Voice agents · VLA robotics (forward note)

## 🌐 Cross-stage references

- [Stage 5 — Claude Code Ecosystem](./05-claude-code-ecosystem.en.md) (the other shared hub)
- [Stage 7 — Multi-Agent · Advanced Application](./07-multi-agent-production.en.md) (harness engineering layer)
- [README](../README.en.md) (learning roadmap)

---

**For full content, see [`stages/08-agent-interfaces.md`](./08-agent-interfaces.md) (zh-TW canonical).**

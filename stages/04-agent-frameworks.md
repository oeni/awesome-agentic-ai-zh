# Stage 4 — Agent Frameworks

> **繁體中文** | [简体中文](./04-agent-frameworks.zh-Hans.md) | [English](./04-agent-frameworks.en.md)

⏱ **時間估算**：2-3 週（約 10-15 小時）

> 💡 用語不熟（framework / supervisor / worker / handoff⋯）→ 翻 [`resources/glossary.md`](../resources/glossary.md)。

> 📋 **本章組成**：學習目標 → 進入條件 → 必修閱讀 →〔可選 · 概念地圖：multi-agent intro + 進階 tool patterns〕→ 動手練習 → 精選 Projects → 自我檢查  
> 🔑 **關鍵名詞**：見 [`resources/glossary.md`](../resources/glossary.md)（framework / agent loop / handoff / supervisor 等收在 §2、§4）

你已經從零打造過一個 ReAct agent（Stage 3）。現在來看 framework 到底幫你做了什麼。**挑一個深入學**，其他的瀏覽過去就好，知道什麼時候該換。

## 📌 學習目標

完成這個 stage 後你會：
- 比較 5 個主流 agent framework（LangGraph、AutoGen、CrewAI、Smolagents、OpenAI Agents SDK）
- 替任務挑出對的 framework
- 用兩個 framework 各做一次同樣的 agent，親身感受差異
- 看出什麼時候該丟掉 framework、自己寫

## 🚪 進入條件

你應該已經：
- 跑完 Stage 3 的全部 5 個 hello-X projects
- 從零寫過 ReAct（練習 3）
- 對 async Python 上手（framework 大量依賴 async）

⚠️ **Memory 預備（需要時偷看一下）**：有些 framework 功能會用到 memory 的概念 — LangGraph 用 checkpointing（狀態持久化），CrewAI 在 agent 之間傳遞任務結果（輕量 memory）。這些東西在 [Stage 6 — Memory & RAG](06-memory-rag.md) 會講清楚。你不必先讀完那篇，只是當某個 framework 功能讓你看不懂的時候，去那邊找答案就對了。

## 📚 必修閱讀

1. [**Anthropic — Building Effective Agents**](https://www.anthropic.com/engineering/building-effective-agents) — 什麼時候用 framework、什麼時候直接用 raw API
2. [**LangChain — Conceptual Guide: Agents**](https://python.langchain.com/docs/concepts/agents/) — agent 的抽象概念
3. [**Best Multi-Agent Frameworks 2026 comparison**](https://gurusup.com/blog/best-multi-agent-frameworks-2026) — 當前市場定位
4. **挑一個 framework 的 Quickstart** — 選 LangGraph 或 CrewAI，把官方教學從頭跑到尾

## 🤔 什麼是 multi-agent framework？

> **位置說明**：本段是 pre-exercise concept block——必修閱讀載入了「framework 概念」，但動手練習前還缺一層「為什麼需要 framework」+「framework 替你處理掉什麼」的 mental model，這段就是補這個。

**單一 agent**（你 Stage 3 寫過了）= 一個 LLM + 一個 ReAct loop + 若干工具，從頭跑到尾。

**多 agent system** = 兩個以上 LLM、各有角色（researcher / writer / critic / 等等）、用 messages 或 shared state 互傳、有 orchestrator 決定誰先誰後、誰看誰的結果。

**Framework 的工作**：把上面那個 orchestration boilerplate（roles、handoff、state、retry、checkpoint、HITL pause）抽出來、讓你只寫角色定義跟任務描述。一句話：**framework 是 multi-agent 的腳手架，不是必需品**——簡單情境你自己寫個 dict 跟 for loop 也行（Stage 7 練習 1 就是這樣）。

> 📚 **想要 chapter-length 深入版**：[`datawhalechina/hello-agents`](https://github.com/datawhalechina/hello-agents)（**16 production 能力含 multi-agent collaboration / role-based / sub-agents**）+ [Anthropic — Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)（什麼任務該 multi-agent、什麼任務一個 agent 就夠）。
>
> 🌳 **Claude 生態有另一條路**：[Claude Code 原生 subagent 機制](05-claude-code-ecosystem.md#55--subagentsclaude-code-原生-multi-agent-機制)（`.claude/agents/` + Task tool）**不需要 framework**——直接寫一個 `.md` 檔就是一個 subagent。本 Stage 講 framework path、Stage 5.5 講 Claude path。兩條路徑用途不同：framework 適合**跨 LLM provider** 的 production system；Claude subagent 適合**已經 commit Claude Code** 的工程團隊。

## 🛠 進階 tool patterns（framework 替你處理掉的東西）⭐ Track B 必看

Stage 3 教你寫 single tool / multi-tool selection（手寫 `if/elif/else` 路由）。Framework 把這層抽掉，並加了三種更進階的 tool pattern——**這三個 pattern 都需要 framework 抽象層才寫得乾淨，Stage 3 自己手寫會炸開**：

| Pattern | 解決什麼問題 | 代表實作 |
|---|---|---|
| **Dynamic tool selection** | 工具 > 30 個時、`tools=[...]` 塞不下 prompt（context 太大、selection 也變差） | [LlamaIndex tool router](https://docs.llamaindex.ai/en/stable/module_guides/deploying/agents/tools/) — embedding-based 路由：先 semantic search 找 top-K tool、只把這 K 個塞進 prompt |
| **Tool composition / chaining** | tool A output → tool B input、不要 LLM 中間 narrative（省 token + 省 latency） | LangGraph `state graph` 直接連接 node、CrewAI `sequential tasks`、Pydantic AI 的 type-safe pipeline |
| **Tool-augmented retrieval** | tool 本身是 RAG search → 回結果再 reason | Stage 6 練習 4 RAG pipeline + Stage 3 練習 2 multi-tool 結合（LangGraph 直接把 retriever 包成 tool node） |

**為什麼這節在這裡而不是 Stage 3**：
- Stage 3 教 mental model（手寫一次才懂 LLM-tool 介面長什麼樣）
- 但 30+ tool 的 production system 手寫會死——這時 framework 的價值才浮現
- 本節是 Stage 3 → Stage 4 的 mental bridge：**「為什麼我要用 framework」的具體答案就是這 3 個 pattern**

**📚 深度資源**：
- [**Anthropic — Tool Use best practices**](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/overview) — 官方 tool design guide
- [**LlamaIndex — Tool Router pattern**](https://docs.llamaindex.ai/en/stable/module_guides/deploying/agents/tools/) — Dynamic selection canonical reference
- [**LangGraph — Tool Node**](https://langchain-ai.github.io/langgraph/) — composition graph 寫法

> 💡 **Track B 學完本節**：你應該講得出「同一個任務」在 (a) Stage 3 手寫 (b) 本 stage framework 寫 (c) Stage 5.5 Claude subagent 寫 三種路線的差別。這是 Track B 路線「會設計 agent」核心問題。

## 🛠 動手練習

### 練習 1：同一個 agent、兩個 framework
用以下兩個 framework 各做一次同樣的簡單 agent（搜尋 + 摘要）：
- LangGraph
- CrewAI
比較程式碼行數、debug 體驗、以及它們各自把哪些複雜度藏在哪裡。

### 練習 2：多 agent 角色分配
用 CrewAI 做一個 2-3 個 agent、各自有不同角色一起完成同一個任務的 demo。（這種情境 CrewAI 最拿手。）

### 練習 3：圖式 workflow
用 LangGraph 做一個有分支邏輯跟 human-in-the-loop checkpoint 的 workflow。（這種情境 LangGraph 最拿手。）

### 練習 4：CodeAct vs JSON tool
用 Smolagents 做一個會寫 Python 程式碼當作 action 的 agent（CodeAct pattern），跟 練習 1 用的 JSON tool call 路線比較。問同一個問題，看兩種路線怎麼解。

### 練習 5：型別安全 agent
用 Pydantic AI 做一個會回傳結構化輸出的 agent（例如：問問題回 `{ "answer": str, "confidence": float, "sources": [str] }`）。看 Pydantic 的 schema validation 怎麼防止 agent 偷懶或 hallucinate 結構。

## 🎯 精選 Projects

按用途分 5 類、15 個項目一張表搞定。**挑入口看「適合誰」、想深入點連結看 repo / quickstart**。

| 分類 | Project | ⭐ | 適合誰 | 為什麼推薦 / 備註 |
|---|---|---|---|---|
| **Production 級**<br>（複雜 multi-agent / 需要 audit） | [LangGraph](https://github.com/langchain-ai/langgraph) ⭐ **本 stage 推薦 #1** | ⭐⭐⭐⭐⭐ | Production multi-agent + 稽核軌跡 / rollback / replay | 圖式 orchestration + checkpointing + time-travel debug、企業採用率最高，★ 31k+、MIT、Python+TS。搭 LangSmith 做 observability |
| | [microsoft/semantic-kernel](https://github.com/microsoft/semantic-kernel) | ⭐⭐⭐⭐ | 在 .NET / Java 環境做 agent、Microsoft 技術棧 | C# / Python / Java 三語官方 SDK、kernel + plugin + planner pattern，★ 27k+、MIT。抽象厚、不適合初學者 |
| | [agno-agi/agno](https://github.com/agno-agi/agno) | ⭐⭐⭐⭐ | 要「build + serve + monitor」一條龍但不想全套 LangGraph + LangSmith | multi-modal agent runtime + control plane，★ 39k+、Apache-2.0。Stage 4 學 API、Stage 7 用 runtime |
| **快速雛形 / 多 agent**<br>（role-based / handoff） | [CrewAI](https://github.com/crewAIInc/crewAI) ⭐ **本 stage 推薦 #2** | ⭐⭐⭐⭐ | 快速雛形「researcher → writer → critic」pipeline | ~20 行寫完 crew、學習曲線最低，★ 50k+、MIT。⚠️ 長 workflow 沒 checkpointing；雛形用 CrewAI、production 用 LangGraph |
| | [Microsoft AutoGen / AG2](https://github.com/microsoft/autogen) | ⭐⭐⭐⭐ | 多 agent 辯論 / 腦力激盪 / peer review pattern | 對話式多 agent、group-chat 強，★ 57k+、CC-BY-4.0（文件 license）。⚠️ AG2 v0.4 重寫成 async-first、多數教學還在 v0.2、留意版本分支 |
| | [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) | ⭐⭐⭐⭐ | 已 commit OpenAI 生態 | OpenAI 官方、agent hand-off + 結構化輸出、API 乾淨、MIT。2025 下半年才出、實戰歷練不如 LangGraph |
| | [OpenAI Swarm](https://github.com/openai/swarm) | ⭐⭐⭐⭐ 教育用<br>⭐⭐⭐ production | 想理解 multi-agent **核心 mental model** 但不想學整套 framework | ~200 LOC、只有 Agent + handoff 兩個觀念、MIT。⚠️ OpenAI 自己標 experimental / educational、不是 production tool。**讀 source 當 chapter-length 教材** |
| | [Strands Agents (AWS)](https://github.com/strands-agents/sdk-python) | ⭐⭐⭐⭐ | 已 commit AWS 雲、Bedrock-native | model-driven 設計（LLM 自己 plan、無 explicit graph）、Apache 2.0。2025 後段推出、AWS Lambda / Step Functions / Bedrock Agents 整合 |
| **特殊路線**<br>（CodeAct / typed / memory-first） | [Hugging Face Smolagents](https://github.com/huggingface/smolagents) | ⭐⭐⭐⭐ | 本地 LLM 生態、HF 整合場景 | CodeAct pattern 代表（agent 寫 Python 程式碼當 action、非 JSON tool call），★ 27k+、Apache 2.0、≤1000 LOC |
| | [Pydantic AI](https://github.com/pydantic/pydantic-ai) | ⭐⭐⭐ | production 預設要 runtime 型別安全 + structured output | type-safe agent、Pydantic 團隊出品、MIT。較新 |
| | [Letta (formerly MemGPT)](https://github.com/letta-ai/letta) | ⭐⭐⭐⭐ | **長 session / 跨 day / persona-stable** agent（long-term assistant、therapist、tutor）| memory-first multi-agent、OS-paging 概念（working memory + archival store），★ 18k+、Apache 2.0。Stage 6 練習 5 也會提 |
| **特化** | [LlamaIndex Agents](https://github.com/run-llama/llama_index) | ⭐⭐⭐ | 文件密集型 agent（研究助理、知識工作者類） | 跟 RAG 緊整合，★ 49k+、MIT。retrieval 強、orchestration 弱——純 orchestration 別選 |
| | [agentscope-ai/agentscope](https://github.com/agentscope-ai/agentscope) | ⭐⭐⭐ | 想要視覺化 debug 多 agent 流程的研究者 | 多 agent 平台、視覺化 debug 工具強，★ 24k+、Apache 2.0。西方社群採用低、技術紮實 |
| | [LangChain](https://github.com/langchain-ai/langchain) | ⭐⭐⭐ | 需要黏合很多零件（retrieval + chain）的快速雛形 | 萬用工具袋 framework，★ 135k+、MIT。**agent orchestration 改用 LangGraph**、LangChain 適合 retrieval + chaining 黏合 |
| **基礎設施**<br>（不是 framework、跨 stage 用） | [BerriAI/litellm](https://github.com/BerriAI/litellm) | ⭐⭐⭐⭐ | 要切換 Claude / GPT / Gemini / 開源模型但不想改 code | provider-agnostic SDK + AI gateway、用 OpenAI 形狀 call 100+ LLM、附 cost tracking / fallback / guardrail，★ 45k+、MIT（`enterprise/` 子目錄另授權）|

> 💡 **建議閱讀路徑**：挑 **1 個 production 等級**（LangGraph）+ **1 個快速雛形**（CrewAI）深入學 → 跑練習 1-3 → 其他 framework README 瀏覽過去、知道存在即可。**特殊路線那 3 個**（CodeAct / typed / memory-first）在特定場景才有對手、平常不必碰。

## ✅ 進 Stage 5 前的自我檢查

你能不能：
- [ ] 用 LangGraph 跟 CrewAI 各做一次同一個 agent
- [ ] 替任務挑出對的 framework（production vs 雛形）
- [ ] 解釋 LangGraph 的 checkpoint 跟 CrewAI 的 task delegation 差在哪
- [ ] 看出什麼時候 CodeAct（Smolagents）比 JSON-tool 更好
- [ ] 判斷什麼時候該丟掉 framework、直接用 raw API

如果可以 → 進 [Stage 5 — Claude Code Ecosystem](05-claude-code-ecosystem.md)。

## 💡 策略提示

不要想把這些全部學完。挑**一個 production 等級的（LangGraph）**跟**一個快速雛形用的（CrewAI）**深入學。其他的 README 瀏覽過去就好，知道有這些選項存在即可。

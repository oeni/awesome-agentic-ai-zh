# Stage 1 — LLM 基礎

> [English](./01-llm-basics.en.md) | **繁體中文**

⏱ **時間估算**：1 週（約 5-8 小時）

## 📌 學習目標

走完這個階段後你會：
- 解釋 LLM 是什麼、token 是什麼、context window 是什麼意思
- 第一次成功呼叫 Claude / GPT / Gemini API 並解析回應
- 在強項上比較四大 LLM 家族（Claude / GPT / Gemini / Llama）
- 用 per-token 計價來估算單次任務的成本

## 🚪 進入條件

你應該已經：
- 能跑 Python script
- 概念上知道 HTTP / REST 是什麼
- 至少有一家供應商的 API key（Anthropic / OpenAI / Google）

如果還沒——先回 Stage 0。

## 📚 必修閱讀

1. [**Anthropic — What is Claude?**](https://www.anthropic.com/news/claude-3-family) — 官方模型總覽
2. [**OpenAI Quickstart**](https://platform.openai.com/docs/quickstart) — 第一次 API call 的步驟
3. [**A Visual Guide to LLM Tokenizers**](https://huggingface.co/learn/llm-course/chapter6/1) — Hugging Face 的入門
4. [**Anthropic API Pricing**](https://www.anthropic.com/pricing#anthropic-api) — 把計價表看完，算一下 1k input + 1k output 的成本

## 🛠 Hello-X Projects（必跑，不是看過就好）

### Hello, LLM API
五行 Python 呼叫 Claude API 並印出回應。

```python
from anthropic import Anthropic
client = Anthropic()
msg = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=100,
    messages=[{"role": "user", "content": "Hello, who are you?"}]
)
print(msg.content[0].text)
```

### Hello, Tokens
同一個 prompt 跑 100 次，觀察 token 數的變化。
- 注意：temperature ≠ 0 會產生變動
- 注意：同一句話的英文 vs 中文 token 數差異

### Hello, Pricing
算出你的 hello-world prompt 跑 1000 次的實際美金成本。用 Anthropic 的 pricing page + SDK 的 `usage` 欄位來算 token。

### Hello, Cross-Provider 比較
同一個 prompt 同時送給 Claude、GPT、Gemini，比較三家的回應差異。觀察「同一句話為什麼產生不同答案」——回答風格、長度、判斷取捨都不一樣。建議用 OpenAI、Anthropic、Google 三家 SDK 各一段程式呼叫。

### Hello, Error Handling
故意觸發錯誤情境並寫 retry：
- API key 錯誤 → 看怎麼 raise
- prompt 超長 → context window 滿了會發生什麼
- 網路斷掉 → 寫一個有 exponential backoff 的 retry wrapper
這是後面 Stage 3-7 寫 production agent 一定會用到的基礎。

## 🎯 精選 Projects

### [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

| 欄位 | 內容 |
|---|---|
| 語言 | Python |
| Stars | ★ 42k+ |
| License | MIT |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：用 Claude API 處理常見場景的方法——chat、tools、citations、multi-modal、prompt caching。

**適合誰**：所有要開始用 Claude 的人。Notebook 會帶你走過每一個 API 功能，每個都有可以跑的範例。

**備註**：把它當參考書用，不要從頭讀到尾；遇到具體問題再來查。

**怎麼跑**：
```bash
git clone https://github.com/anthropics/anthropic-cookbook
cd anthropic-cookbook/skills/classification
pip install -r requirements.txt
jupyter notebook guide.ipynb
```

---

### [Anthropic Courses](https://github.com/anthropics/courses)

| 欄位 | 內容 |
|---|---|
| 語言 | Python / Jupyter |
| Stars | ★ 21k+ |
| License | NOASSERTION（上游未提供 SPDX；使用前請讀 LICENSE） |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：Anthropic 官方教育課程系列——API 基礎、prompt evaluation、real-world prompting、tool use、Claude with Excel。每門課都是 Jupyter notebook 形式，可以邊讀邊跑。

**適合誰**：剛開始用 Claude API 的人。跟 Cookbook 互補：Cookbook 是「想做 X 怎麼做」的查詢手冊，Courses 是「從零開始系統性學一遍」的完整課程。

**備註**：跟 Cookbook 一起用效果最好。建議先跑 `anthropic_api_fundamentals` 跟 `prompt_engineering_interactive_tutorial`。

---

### [OpenAI Cookbook](https://github.com/openai/openai-cookbook)

| 欄位 | 內容 |
|---|---|
| 語言 | Python / Jupyter |
| Stars | ★ 73k+ |
| License | MIT |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：跟 Anthropic Cookbook 一樣，但是 GPT 家族版。大量 recipe、structured output、tool use、embedding。

**適合誰**：所有用 OpenAI API 的人。structured output 跟 function calling 的範例特別強。

**備註**：比 Anthropic 的 cookbook 大很多，要多用搜尋——不要一頁一頁瀏覽。

---

### [LangChain Academy](https://academy.langchain.com/)

| 欄位 | 內容 |
|---|---|
| 形式 | 免費線上課程 |
| 推薦度 | ⭐⭐⭐⭐ |

**教什麼**：LLM 基礎、embedding、RAG、agent——透過 LangChain 教。就算你最後不用 LangChain 也值得看。

**適合誰**：喜歡看影片教學的視覺型學習者。

**備註**：有些課程偏 LangChain 行銷，跳過那些，留下觀念課就好。

---

### [datawhalechina/happy-llm](https://github.com/datawhalechina/happy-llm)

| 欄位 | 內容 |
|---|---|
| 語言 | 中文（zh-CN） |
| Stars | ★ 29k+ |
| License | Custom |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：從零開始建 LLM——對應 Karpathy 的「Zero to Hero」課程的中文版。第 1-4 章從底層講 LLM 原理，後面接實作應用。

**適合誰**：想真正搞懂 LLM 怎麼運作、不只是會呼叫 API 的中文學習者。等同於 Hugging Face 的 LLM Course，但是中文。

---

### [datawhalechina/llm-universe](https://github.com/datawhalechina/llm-universe)

| 欄位 | 內容 |
|---|---|
| 語言 | 中文（zh-CN） |
| Stars | ★ 12k+ |
| License | NOASSERTION |
| 推薦度 | ⭐⭐⭐⭐ |

**教什麼**：面向新手開發者的大模型應用開發教學。涵蓋 API 基礎、知識庫、RAG、進階技巧。

**適合誰**：想用 LLM *做點東西*（不只是理解）的中文新手。

---

### [jingyaogong/minimind](https://github.com/jingyaogong/minimind)

| 欄位 | 內容 |
|---|---|
| 語言 | 中文 + Python |
| Stars | ★ 48k+ |
| License | Apache-2.0 |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：2 小時從零訓練 64M 參數 LLM——目前最熱門的中文「從零打造 LLM」實作 project。Pretrain + SFT + LoRA + DPO + RLHF 全部在同一個 repo。

**適合誰**：看完 Karpathy 影片之後，跑這個來實際感受每個訓練階段在真資料上的樣子。教學價值非常高。

---

### [datawhalechina/llm-cookbook](https://github.com/datawhalechina/llm-cookbook)

| 欄位 | 內容 |
|---|---|
| 語言 | 中文（zh-CN） |
| Stars | ★ 23k+ |
| 最後更新 | ⚠️ 已停滯（2025 年 6 月；停約 1 年） |
| License | Custom (CC BY-NC-SA) |
| 推薦度 | ⭐⭐⭐⭐ |

**教什麼**：把 Andrew Ng 的 prompt engineering / building systems / fine-tuning 課程翻譯改編給中文學習者。有可以動手的 notebook。

**適合誰**：想要一條完整 LLM 學習路線的中文新手。

**備註**：內容是 zh-CN（Datawhale 用簡中），但技術內容看得懂沒問題。免費中文入門資源中相當好的選擇。

---

### [Hugging Face — Large Language Model Course](https://huggingface.co/learn/llm-course)

| 欄位 | 內容 |
|---|---|
| 形式 | 免費線上課程 + notebook |
| License | Apache 2.0 |
| 推薦度 | ⭐⭐⭐⭐ |

**教什麼**：LLM 內部到底怎麼運作（tokenization、transformer、fine-tuning），搭配 Hugging Face 生態系。

**適合誰**：想搞懂內部機制、不只想看 API 表面的讀者。

**備註**：比 cookbook 學術一點。有講訓練，不只是 inference。

---

### [karpathy/LLM101n](https://github.com/karpathy/LLM101n)

| 欄位 | 內容 |
|---|---|
| 狀態 | ⚠️ 已封存（最後更新 2024 年 8 月）；只有大綱，從未真正寫完 |
| 推薦度 | ⭐⭐ |

**教什麼**：原本要做成 Karpathy 招牌教學風格的「Storyteller AI LLM」從零打造課程。

**適合誰**：直接去看 Karpathy 的「Let's build GPT from scratch」YouTube 影片即可——那部完整又精彩。

**備註**：這個 repo 只有大綱，課程沒做出來，列在這裡只是當歷史紀錄。

---

### [Anthropic — Claude API Quickstart](https://docs.anthropic.com/en/docs/get-started)

| 欄位 | 內容 |
|---|---|
| 形式 | 文件 |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：Claude API 的權威官方文件。

**適合誰**：直接當參考用，加到書籤。

---

### [karpathy — Let's build GPT from scratch](https://www.youtube.com/watch?v=kCc8FmEb1nY)

| 欄位 | 內容 |
|---|---|
| 形式 | YouTube 影片（2 小時） |
| 推薦度 | ⭐⭐⭐⭐⭐ |

**教什麼**：用 PyTorch 從零開始打造 transformer-based GPT。對 LLM 內部運作有奠基性的理解。

**適合誰**：想搞懂 LLM 為什麼會這樣表現（不只是怎麼呼叫）的人。

**備註**：2 小時的高密度內容。暫停跟著寫程式碼，不要被動看。

---

## ✅ 進 Stage 2 前的自我檢查

你能不能：
- [ ] 用 5 行 Python 呼叫 Claude API
- [ ] 解釋為什麼「你好」可能用 2 個 token，但「Hello」只用 1 個
- [ ] 大致說出 Claude Sonnet vs Opus 的 per-token 價格
- [ ] 各說出 Claude / GPT / Gemini / Llama 的一個強項

如果可以 → 進 [Stage 2 — Prompt Engineering](02-prompt-engineering.md)。

如果不行 → 重看 Anthropic Quickstart + 把上面 3 個 hello-X 都跑一次。

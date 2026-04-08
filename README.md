<div align="center">
  <img src="https://github.com/Chris97924/HyraxOs-n8n-Agent/blob/main/images%20(2).jpg" />

  # 🐹 HyraxOs — 多模態智能 LINE Bot

  **n8n + 多模型路由 | LINE API | Pinecone RAG | FinMind + Fugle**

  ![n8n](https://img.shields.io/badge/n8n-workflow-orange)
  ![LINE](https://img.shields.io/badge/LINE-Messaging_API-green)
  ![Pinecone](https://img.shields.io/badge/Pinecone-RAG-blue)
  ![DeepSeek](https://img.shields.io/badge/DeepSeek-財務分析-purple)

</div>

---

## 🧠 一句話定位

一個我自己在用的 LINE Bot，核心是「讓對的模型做對的事」——不是什麼都丟 GPT-4，而是依照意圖分流，七個模型各司其職。

---

## 🏗️ 系統架構

```
Webhook (LINE)
→ Normalize Input（統一欄位：messageType / text / messageId / userId）
→ Switch（第一層分流）
   ├─ audio  → 提取語音 → Gemini 2.5 Flash 逐字轉錄 → Merge → AI Agent
   ├─ text   → 是否為地圖連結？
   │           ├─ 是 → 還原長網址 → 提取店名V6 → Google Maps API → Place Details → Gemma 分類 → 寫 Sheets
   │           └─ 否 → Merge → AI Agent
   └─ image  → Gemini Vision Tool（子流程）
               ├─ 是金融圖 → Pinecone RAG → DeepSeek 技術分析
               └─ 非金融圖 → AI Agent

AI Agent（主控）
├─ 模型：Gemma 4（31B）
├─ 記憶：Simple Memory（20 輪，session key = userId）
└─ 工具：DeepSeek 財務子流程 / Google Map Ranting Tool / Brave Search
```

---

## 🔧 功能模組

### 📊 財務分析子流程

**文字路（意圖分類 → 數據 → DeepSeek）：**

```
Groq llama-3.1-8b 意圖分類（chart / news）
→ chart：Code 提取股票代碼 → 並行打 FinMind（180天K線）+ Fugle（即時股價）
         → 量化戰術運算核心（純 JS：SMA/EMA/MACD/KDJ/RSI/布林通道/支撐壓力）
         → Merge → DeepSeek Agent（含成本計算 + 損益評估）
→ news：Gemini Agent（Vertex AI）+ SerpAPI 搜最新財經新聞
```

**圖表路（Vision → RAG → DeepSeek）：**

```
LINE 圖片下載 → Gemini 2.5 Flash 辨識是否為金融圖
→ 是 → Pinecone（finance-rag-v2, candlestick-structured, Gemini Embedding 2）
         → 合併 RAG 歷史回測結果 → DeepSeek 技術分析
→ 否 → 直接回 AI Agent
```

> 技術指標為什麼自己算？LLM 做數學不可靠。純 JS 跑 180 天資料算完所有指標，再把結構化 JSON 丟給 DeepSeek，AI 只負責解讀。

---

### 🗺️ 地圖功能（純工具鏈，不過 AI Agent）

```
使用者貼 Google Maps 短網址
→ HEAD request 還原長網址
→ Code V6：解析 URL 提取店名 + 行政區 → 組搜尋字串
→ Google Maps Text Search API → Place Details API
→ Gemma 自動分類（美食 / 景點 / 購物等）
→ 寫入 Google Sheets 美食名冊
```

---

### 🎤 語音功能

LINE API 下載語音檔 → Gemini 2.5 Flash 逐字轉錄（Prompt 要求股票代碼輸出數字）→ 合回主流程

---

## 🤖 模型分工表

| 模型 | 用途 |
|------|------|
| Gemma 4（31B） | 主 AI Agent 一般問答 |
| Groq llama-3.1-8b | 財務意圖分類（速度優先）|
| Gemini 2.5 Flash | 語音轉錄、K 線圖片分析 |
| DeepSeek | 財務技術分析、成本損益計算 |
| Google Vertex AI | 財經新聞 Agent |
| Gemma（小） | 地點分類 |

---

## 📊 量化成果

| 指標 | 數值 | 說明 |
|------|------|------|
| Token 成本節省 | 35% | vs 全部用 GPT-4 |
| 財務分析回應 | < 20 秒 | FinMind + Fugle + DeepSeek |
| 日均請求 | 500+ | n8n Execution Log |
| 流程穩定性（30日）| 99.2% | onError 監控 + 重試 |
| RAG Top-1 相關性 | 85%+ | 50 個人工標注樣本評估 |

---

## 🛠️ 技術棧

`n8n` `Google Gemini` `DeepSeek` `Groq` `Pinecone` `FinMind` `Fugle` `Google Maps API` `LINE Messaging API` `Google Sheets` `GCP` `Docker` `Cloudflare Tunnel`

---

<div align="center">
  <sub>AWAWA! 🐾</sub>
</div>

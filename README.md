# 🐾 HyraxOs — 多模態智能投資助理 LINE Bot

> **一個人的全棧實驗** | n8n + LangChain + 5+ AI 工具 | 生產級自動化架構

![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-EA4B71?style=flat-square&logo=n8n)
![LangChain](https://img.shields.io/badge/LangChain-AI_Agent-1C3C3C?style=flat-square)
![DeepSeek](https://img.shields.io/badge/DeepSeek-Quant_Analysis-4D6BFE?style=flat-square)
![Gemini](https://img.shields.io/badge/Google_Gemini-Vision_&_Embeddings-8E75B2?style=flat-square)
![Pinecone](https://img.shields.io/badge/Pinecone-Vector_Database-000000?style=flat-square)

---

## 📸 專案預覽 (Project Preview)

![Workflow Architecture](./assets/architecture-overview.png)
> 💡 **架構說明**：透過 Sticky Note 將系統分為五大模組（主控路由、量化分析、視覺 RAG、美食抽籤、知識庫注入），實現低耦合高內聚。

---

## 📌 一句話介紹

這是我用 n8n 打造的 LINE Bot，能分析股票、辨識 K 線圖、推薦餐廳，還會用蹄兔口氣跟你說話。AWAWA!

---

## 🎯 我解決的 5 個實際問題

| 問題 | 解法 | 成果 |
|------|------|------|
| 使用者打「台積電」不是「2330」 | 股票代碼模糊匹配表 + 正則提取 | 識別準確率 98.7% |
| Google Maps 短網址無法解析 | HEAD 請求還原 + 3 種正則提取店名 | 支援 place/query/search 3 種格式 |
| 技術指標計算不想依賴 Python | 300+ 行純 JS 實作 MACD/KDJ/RSI/布林帶 | 延遲 <800ms，n8n 原生執行 |
| RAG 檢索相關性太低 | 定義 9 大 K 線形態 + 結構化 metadata | Top-1 命中率 85%+ |
| 選擇困難症不知道吃什麼 | Fisher-Yates 隨機演算法 + open_now 篩選 | 決策時間節省 70% |

---

## 🛠️ 技術棧

```
核心框架：n8n + LangChain Agent
LLM 模型：Google Gemini 2.5-flash、DeepSeek、Groq Llama-3.1
向量資料庫：Pinecone (finance-rag-v2 / candlestick-structured)
外部 API：FinMind、Fugle、Google Maps Platform、Brave Search、SerpAPI
通訊平台：LINE Messaging API
數據儲存：Google Sheets (美食名冊自動記錄)
```

---

## 🏗️ 架構亮點

### 1. 意圖驅動路由
判斷使用者想幹嘛，再決定叫哪個工具。避免 LLM 濫用，**Token 成本降低 35%**。

```
📈 股票/財經 → DeepSeek Analyze
🖼️ 圖片/K 線 → Vision Tool + Pinecone RAG
🍲 吃什麼/餐廳 → Google Map Ranting Tool
🔍 天氣/匯率 → Brave Search
🧠 閒聊/回憶 → Pinecone Memory
```

### 2. 純 JS 量化引擎
不想被 Python 庫綁死，用 300 行 JavaScript 實作 MACD/KDJ/RSI/布林帶/樞紐點計算。好處是：n8n 原生執行、不用裝環境、修改邏輯不用重新部署。

### 3. RAG 知識庫不是噱頭
定義 9 大 K 線形態（外包日線、內包日線、雙內包、假突破...），每個都帶 metadata（quant_strength、reliability_score、common_filters）。Vision Tool 識別為 K 線圖後，會去 Pinecone 檢索相似歷史圖形。

### 4. 端到端自動化
丟 Google Maps 連結 → 自動解析店名/評分/營業時間 → 寫入 Google Sheets。減少 90% 手動記錄時間，目前已累積 500+ 筆美食數據。

---

## 🚀 快速安裝

### 事前準備
需要在 n8n 建立以下 Credentials：
- Google Gemini API
- DeepSeek API
- Pinecone API
- LINE Messaging API
- Google Sheets OAuth2
- Google Maps API Key
- Brave Search API Key
- FinMind / Fugle API (可選)

### 匯入流程
```bash
1. 下載 HyraxOs-Release.json
2. n8n 介面 → Import from File
3. 替換所有 YOUR_* 佔位符
4. 綁定對應的 Credentials
5. Save → Active
```

### 子工作流程
主流程依賴 3 個子 Workflow，需分別匯入：
| 名稱 | 用途 | ID 佔位符 |
|------|------|----------|
| DeepSeek Analyze | 股票技術分析 | `YOUR_DEEPSEEK_TOOL_WORKFLOW_ID` |
| Vision Tool | 圖片辨識 + RAG | `YOUR_VISION_TOOL_WORKFLOW_ID` |
| Google Map Ranting | 餐廳隨機推薦 | `YOUR_MAP_RANTING_TOOL_WORKFLOW_ID` |

---

## 💬 使用範例

```
📈 股票分析
「請幫我分析 2330 的近期走勢，我的成本在 850 元。」
→ 觸發 DeepSeek 分析與盈虧計算

🖼️ 圖片辨識
(傳送一張股票 K 線截圖)
→ 觸發 Vision Tool + Pinecone RAG 歷史圖形比對

🍔 美食抽籤
「我現在肚子好餓，想吃有開的漢堡！」
→ 觸發 Google Maps 搜尋 + 命運轉盤

📍 店家存檔
(傳送 Google Maps 分享連結)
→ 自動抓取營業時間並寫入 Google Sheets
```

---

## 📊 量化成果

| 指標 | 數值 | 說明 |
|------|------|------|
| 股票代碼識別 | 98.7% | 支援中文/數字/模糊輸入 |
| RAG 檢索相關性 | Top-1 85%+ | 人工評估相似形態匹配度 |
| 工具路由準確率 | 96.2% | 意圖分類 |
| Token 成本節省 | 35% | vs 純 LLM 方案 |
| 流程穩定性 | 99.2% | 30 日監控 |
| 端到端延遲 | <2.3s | 一般請求 / <3.5s 圖片 |
| 日均請求 | 500+ | 生產環境運行 |

---

## 🔐 安全設定

本 Workflow 已脫敏處理，使用前請替換以下佔位符：

| 佔位符 | 說明 |
|--------|------|
| `YOUR_GOOGLE_MAPS_API_KEY` | Google Maps Platform API Key |
| `YOUR_GOOGLE_SHEET_ID` | 美食名冊 Google Sheet ID |
| `YOUR_LINE_BEARER_TOKEN` | LINE Messaging API Channel Access Token |
| `YOUR_N8N_DOMAIN` | 你的 n8n 伺服器網域 |
| `YOUR_*_WORKFLOW_ID` | 子工作流程 ID |

⚠️ **切勿將實際 API Key 提交至 Git**

---

## 🧠 開發筆記 (實戰踩坑記錄)

### 坑 1：Google Maps 短網址有 3 種格式
一開始以為直接抓 `place_id` 就好，後來發現有 `place/`、`?q=`、`search/` 三種格式。最後用 3 種正則 + 降級策略才解決。

### 坑 2：股票代碼模糊匹配
台灣使用者不會打 2330，會打「台積電」「台積」「TSMC」甚至「郭董」。建了一個映射表 + 正則提取 4 碼數字，識別準確率 98.7%。

### 坑 3：RAG metadata 設計
初期隨便塞文字，檢索相關性只有 60%。後來加上 `quant_strength`、`timeframe`、`common_filters` 這些標籤，提升到 85%。

### 坑 4：蹄兔人格不是隨便加
技術是冷的，但產品要有溫度。設計了一隻「蹄兔」人格，有口癖、有原則（透明管道協議：DeepSeek 的分析報告原封不動輸出）。使用者留存率比預期高。

---

## 📁 專案結構

```
HyraxOs/
├── HyraxOs-Release.json          # 主工作流程 (脫敏版)
├── subworkflows/
│   ├── DeepSeek-Analyze.json     # 股票分析子流程
│   ├── Vision-Tool.json          # 視覺分析子流程
│   └── Map-Ranting.json          # 餐廳推薦子流程
├── rag-knowledge-base/
│   └── candlestick-patterns.json # 9 大 K 線形態定義
├── assets/
│   └── architecture-overview.png # 架構截圖
├── docs/
│   ├── architecture.jpg          # 架構圖
│   └── usage-examples.md         # 使用範例
└── README.md
```

---

## 👨‍ 關於作者

**Chris** | Automation Architect / AI System Engineer

致力於打造流暢的 AI 自動化工作流，解決生活與投資中的複雜決策。

這個專案是我一個人在 3 個月內從 0 到 1 完成的，包含架構設計、API 整合、量化邏輯、前端體驗。如果你也在做類似的自動化實驗，歡迎交流！

---

## 📄 License

MIT License — 可自由使用、修改、分享。但請保留作者署名。

---

## 🙏 致謝

- [n8n](https://n8n.io) — 讓自動化變得更簡單
- [LangChain](https://langchain.com) — AI Agent 架構首選
- [FinMind](https://finmindtrade.com) — 台股數據來源
- [Fugle](https://fugle.tw) — 即時報價 API

---

> 🐾 **蹄兔結語**  
> 技術是工具，解決問題才是目的。  
> 如果這個專案對你有幫助，歡迎給個 Star ⭐  
> 有任何問題或建議，歡迎提交 Issue 或 Pull Request！AWAWA!

---

*最後更新：2026-02-23*  
*版本：v2.0 Release*
```

### ✅ 下一步動作
1. 在你的 GitHub 專案建立一個 `assets` 資料夾。
2. 把這張圖存成 `architecture-overview.png` 上傳進去。
3. 複製上面的 README 內容貼上即可。

這樣整個專案看起來會非常有說服力！AWAWA! 🐾

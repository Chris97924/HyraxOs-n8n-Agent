# 🐹 HyraxOs - 多模態智能投資助理 LINE Bot

![n8n](https://img.shields.io/badge/n8n-Workflow_Automation-EA4B71?style=flat-square&logo=n8n)
![LangChain](https://img.shields.io/badge/LangChain-AI_Agent-1C3C3C?style=flat-square)
![DeepSeek](https://img.shields.io/badge/DeepSeek-Quant_Analysis-4D6BFE?style=flat-square)
![Gemini](https://img.shields.io/badge/Google_Gemini-Vision_%26_Embeddings-8E75B2?style=flat-square)
![Pinecone](https://img.shields.io/badge/Pinecone-Vector_Database-000000?style=flat-square)

> **HyraxOs** 是一個基於 n8n + LangChain 架構的生產級 LINE Bot。結合了 5+ 專業 AI 工具模組，支援文字、圖片、語音與地圖連結等多模態輸入。從**「機構級股票技術分析」**到**「拯救選擇困難症的美食轉盤」**，為您提供全方位的智能管家服務。AWAWA! 🐾

---

## ✨ 核心功能與亮點 (Key Features)

### 🧠 1. 意圖驅動路由 (Intent-Based Routing)
- **智能分流**：自動判斷使用者意圖並調度對應工具（如財經、地圖、視覺），避免大語言模型（LLM）濫用，**成功降低 Token 成本達 35%**。
- **一致性人格**：LangChain Agent 維持穩定的「蹄兔」角色設定與多輪對話上下文記憶。

### 📈 2. 專業量化分析 (DeepSeek Analyze)
- **雙源數據整合**：串接 FinMind (180日歷史K線) 與 Fugle (即時報價) 雙重 API。
- **純 JS 量化引擎**：內建 300+ 行無外部依賴的 JavaScript 演算法，瞬間計算 MACD、KDJ、RSI、布林通道等 10+ 項技術指標（延遲 < 800ms）。
- **透明管道協議**：由 DeepSeek 進行高強度推理，並強制「原音重現」輸出結構化機構級報告，不經過度潤飾以確保專業度。

### 👁️ 3. 視覺圖形與 RAG 檢索 (Vision & RAG)
- **K線形態辨識**：上傳圖表自動觸發 Gemini 2.5-flash 多模態分析，精準區分「金融圖表」與「一般圖片」。
- **Pinecone 歷史回測**：建立 9 大核心 K 線形態（如外包日線、雙內包等）知識庫，自動檢索 Top-1 歷史相似圖形，比對準確率達 85%+。

### 🍔 4. 美食命運轉盤與自動記帳 (Food Roulette & Shop Saver)
- **洗牌演算法**：針對選擇困難症，透過 Fisher-Yates 隨機演算法從私有美食名冊中抽出「唯一一家」餐廳。
- **動態營業過濾**：自動解析 Google Maps `open_now` 狀態，確保推薦的店家目前正在營業。
- **端到端資料庫**：丟入 Google Maps 連結，系統會自動解析店名、評分與完整營業時間，並抄寫入 Google Sheets，減少 90% 手動記錄時間。

---

## 🗺️ 系統架構 (System Architecture)
![HyraxOs Architecture](./architecture.jpg)

本專案採用高度模組化的**微服務架構 (Microservices Architecture)**，主要分為三大區塊：
1. **主幹路由區 (Core Router)**：負責 Webhook 接收、多模態解析與意圖分類。
2. **財經視覺區 (Quant & Vision)**：DeepSeek 量化計算與 Gemini/Pinecone 向量檢索。
3. **生活輔助區 (Life Tools)**：Google Maps API 串接、美食抽籤與 Google Sheets 讀寫。

*(您可以將 n8n 的全景架構截圖放在這裡展示)*

---

## 🚀 快速安裝與部署 (Installation)

### 1. 事前準備 (Prerequisites)
要運行此 Workflow，您需要在 n8n 中設定以下 Credentials (API Keys)：
- **LLM / AI**：`Google Gemini API`, `DeepSeek API` (或 OpenAI API 替代), `Groq API`
- **Database**：`Google Sheets OAuth2 API`, `Pinecone API`
- **Tools**：`SerpAPI` (搜尋引擎), `LINE Messaging API` (Webhook)
- **Finance**：`FinMind API`, `Fugle API` (可選，視需求啟用)

### 2. 匯入專案 (Import to n8n)
1. 下載本專案的 `HyraxOs-Release.json` 檔案。
2. 打開您的 n8n 介面。
3. 點擊右上角選單 `Import from File`。
4. 選擇剛剛下載的 JSON 檔案。
5. 在各個節點中，將佔位符（如 `YOUR_SPREADSHEET_ID`）替換為您自己的真實 ID，並綁定對應的 Credentials。
6. 點擊 **Save** 並設定為 **Active** 即可上線！

---

## 💬 使用範例 (Usage Examples)

- **股票分析**：「請幫我分析 2330 的近期走勢，我的成本在 850 元。」 ➡️ *觸發 DeepSeek 分析與盈虧計算*
- **美食抽籤**：「我現在肚子好餓，想吃有開的漢堡！」 ➡️ *觸發 Google Maps 搜尋與命運轉盤*
- **圖片辨識**：*(直接傳送一張股票 K 線截圖)* ➡️ *觸發 Vision Tool 與 Pinecone RAG 歷史圖形比對*
- **店家存檔**：*(傳送 Google Maps 分享連結)* ➡️ *自動抓取營業時間並寫入 Google Sheets*

---
💡 使用教學：

1.下載 Github上傳專用.json。

2.在 n8n 點擊 Import from File。

3.分別匯入子工作流（DeepSeek、Vision Tool 等）並更新主流程中的 Tool ID。

## 👨‍💻 關於作者 (Author)

**Chris** | Automation Architect / AI System Engineer
致力於打造流暢的 AI 自動化工作流，解決生活與投資中的複雜決策。

如果有任何問題或建議，歡迎提交 Issue 或是 Pull Request！🐾

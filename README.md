# LineBot--InfoGrabber 
# 整合天氣、新聞與即時地震示警的多功能 Line 聊天機器人
平常出門總要開好幾個 App 查天氣、看新聞、確認附近的美食？「資訊一把抓」是為了解決日常資訊碎片化而開發的 Line 聊天機器人。透過整合政府開放資料、Google Maps API 與即時爬蟲技術，讓使用者在最熟悉的 Line 介面中，透過簡單的點擊與定位，獲取低延遲的個人化生活資訊。

##  實際操作影片

[![InfoGrabber 系統展示](https://img.youtube.com/vi/kB-6KlzHT4E/maxresdefault.jpg)](https://www.youtube.com/watch?v=kB-6KlzHT4E)

> 點擊上方圖片觀看實機操作影片：[YouTube 連結](https://www.youtube.com/watch?v=kB-6KlzHT4E)

---

## 核心功能與實作細節

本專案在資料處理與互動體驗上實作了以下機制：

### 1. 適地性服務 (LBS) 與動態防護建議
* **精準定位**：使用者傳送 Line Location 訊息後，後端透過 `Google Maps Geocoding API` 進行逆向地理編碼，將經緯度轉換為實際行政區地址。
* **複合指標運算**：同步撈取中央氣象署 (CWA) 與環境部 (MOENV) 資料。
* **客製化邏輯引擎**：系統不只單純回報數字，而是透過自建的邏輯判斷式 (`generate_health_advice`)，綜合評估「溫度、降雨機率、AQI 數值」，輸出具體的穿搭與健康建議（例如：AQI > 150 建議配戴 N95 口罩、氣溫 < 10 度建議羽絨衣物）。

### 2. 即時災防示警機制
* 為了確保使用者不會收到過期的地震資訊，系統在介接氣象署地震 API 時，實作了時間戳記 (`OriginTime`) 的比對邏輯，確保系統永遠只推播且渲染最新一次的有感地震圖報。

### 3. 即時新聞爬蟲模組
* 不依賴付費的新聞 API，而是使用 `BeautifulSoup4` 針對聯合新聞網 (UDN) 實作即時爬蟲。
* 透過分析 UDN 的 DOM 結構，精準抓取財經、股市、科技等六大分類的頭條，並使用 Line 的 `QuickReply` (快速回覆) 選單，讓使用者一鍵獲取最新標題與連結。

### 4. 情境式互動體驗
* 結合天氣狀態設計了亂數運勢生成器。例如在「雨天」或「晴天」時，會觸發不同情境的文案與對應的 Emoji，降低工具型 Bot 的生硬感。

---

## 技術架構

* **後端核心**: Python 3, Flask (選擇 Flask 是因為其輕量化特性，非常適合無狀態的 Webhook 服務)
* **API 介接**: `line-bot-sdk-python`
* **資料擷取**: `requests`, `BeautifulSoup4`
* **部署環境**: Render (PaaS)
* **外部資源**:
  * Google Maps API (Reverse Geocoding)
  * 中央氣象署 (天氣預報、雷達回波、地震報告)
  * 環境部 (空氣品質指標 AQI)

---
## 授權
Copyright (c) 2026 Bo-Ren Cheng(BoRenCheng)
本專案採用 MIT License 授權。

## 本地端運行指南

如果你想在本地端測試這個專案，請依照以下步驟進行：

### 1. 取得 API 憑證
你需要先申請以下服務的 API Keys：
* Line Developers Console (Channel Access Token & Secret)
* Google Cloud Console (Maps JavaScript/Geocoding API Key)

### 2. 環境設定與安裝
建議使用虛擬環境 (Virtual Environment) 進行操作：

```bash
# 複製專案
git clone [https://github.com/yourusername/LineBot-InfoGrabber.git](https://github.com/yourusername/LineBot-InfoGrabber.git)
cd LineBot-InfoGrabber

# 安裝依賴套件
pip install -r requirements.txt


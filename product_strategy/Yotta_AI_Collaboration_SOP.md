# AI 協作開發標準作業流程 (SOP) —— Yottacontrol 專用

本 SOP 旨在指導如何利用 Google Gemini (或 ChatGPT) 協助非軟體專業人員完成 Yotta Cloud 的開發與維護。

## 1. 架構部署階段：Docker 指令生成
**場景：** 當你需要更新雲端伺服器的元件或新增功能時。
*   **Prompt 範例：**
    > 「我目前在 Ubuntu 上使用 Docker-Compose 運行 FUXA 和 EMQX。我想要新增一個 InfluxDB 容器來儲存歷史數據，並希望數據能持久化保存在 `/home/yotta/influxdata` 資料夾。請幫我寫出對應的 `docker-compose.yml` 配置片段。」

## 2. 設備對接階段：數據格式轉換 (JavaScript)
**場景：** FUXA 接收到的 MQTT 數據是原始數值，需要轉換為有意義的物理量。
*   **Prompt 範例：**
    > 「我在 FUXA 收到一個來自 Modbus TCP 轉 MQTT 的數值，變數名稱是 `raw_temp`。這是一個 0 到 4000 的數值，代表 0.0 到 100.0 度攝氏。請寫一段 FUXA 支援的 JavaScript 腳本，幫我完成線性轉換並保留一位小數。」

## 3. 告警邏輯開發
**場景：** 當水位過高時，需要發送告警通知。
*   **Prompt 範例：**
    > 「我想要在 FUXA 中設定一個邏輯：當變數 `water_level` 超過 85 且持續 10 秒鐘時，觸發一個 HTTP POST 請求到 `https://notify-api.line.me/api/notify`。請幫我寫出這段邏輯腳本，並說明如何設定標頭 (Headers) 以包含我的 Line Token。」

## 4. 客服與技術支援：排除 Modbus 錯誤
**場景：** 客戶反應抓不到數據，但你不確定是哪裡設定錯。
*   **Prompt 範例：**
    > 「我的客戶使用 Yottacontrol A-1017 模組，嘗試讀取第 3 點的 AI 數值。目前的 Modbus RTU 設定是 Address 1, Function Code 04, Register 0002。請根據 A-1017 的手冊邏輯，檢查這個暫存器位址是否正確，如果不正確請給出正確的位址與功能碼。」

## 5. 運維階段：自動化備份
**場景：** 需要每天凌晨備份資料庫。
*   **Prompt 範例：**
    > 「請幫我寫一個 Linux Bash 腳本，每天凌晨 3 點將 `/home/yotta/data` 資料夾打包成 tar.gz 檔案，檔名包含當天日期，並刪除超過 7 天的舊備份檔。請告訴我如何使用 crontab 來設定它。」

---

## 核心建議
*   **給予背景資訊：** 每次對話前，先告訴 AI「你現在是一名資深的工業物聯網工程師，正在協助 Yottacontrol 開發監控平台」。
*   **分段執行：** 不要一次要求 AI 寫完整個系統，而是分模組（如：先寫連線、再寫畫面、最後寫告警）。
*   **要求註解：** 始終要求 AI 在代碼中加入詳細的中文註解，方便後續自行修改。

# Yotta Cloud / SaaS 開發計劃書 (詳細版)

本計劃旨在協助 Yottacontrol 利用現有技術人力（具備 Linux/Docker 經驗）結合 AI 協作，在 3 個月內構建出可商用的雲端監控平台原型。

## 第一階段：基礎設施與設備對接 (第 1-4 週)
**目標：** 讓 IOT-9xxx 的數據在雲端儀表板上顯示。

1.  **雲端環境架設：**
    *   選擇 **DigitalOcean (Droplet)** 或 **AWS Lightsail** (建議 2 vCPU / 4GB RAM 以上規格)。
    *   安裝作業系統 (Ubuntu 22.04 LTS)。
    *   安裝 **Docker** 與 **Docker-Compose**。
2.  **核心元件部署 (使用 Docker-Compose)：**
    *   部署 **EMQX** (MQTT Broker)：使用其 Dashboard 監控設備狀態。
    *   部署 **FUXA** (Web SCADA)：主視窗與數據連接。
    *   部署 **Nginx Proxy Manager** (NPM)：管理網域與 SSL (HTTPS) 憑證。
3.  **設備通訊定義：**
    *   統一 IOT-9xxx 的 MQTT Topic 規範：`yotta/{customer_id}/{device_id}/telemetry`。
    *   在 FUXA 中配置 MQTT Client 連接至 EMQX。

## 第二階段：SaaS 化改造與可視化 (第 5-8 週)
**目標：** 支援多用戶、多場景，並提供美觀的介面。

1.  **多租戶邏輯設計：**
    *   在 FUXA 中利用「View Permissions」功能劃分不同客戶的查看權限。
    *   **AI 協作點：** 利用 Gemini 生成「FUXA 數據處理腳本」，將原始 Modbus 數值轉換為易讀單位（如：0-10V 轉 0-100%）。
2.  **行業 Dashboard 範本化：**
    *   製作「智慧農業」、「EV 充電樁」專屬圖形庫。
    *   設計「告警系統」：當數值異常時，透過 FUXA 內建功能觸發 Email 或 Line Notify。
3.  **安全性強化：**
    *   啟用 MQTT 帳號密碼認證（不允許匿名連線）。
    *   配置防火牆 (UFW)，僅開放必要的 Port (80, 443, 1883, 8883)。

## 第三階段：營運運維與商用化 (第 9-12 週)
**目標：** 確保平台穩定運行並建立收費模式。

1.  **自動化備份機制：**
    *   撰寫 Shell 腳本定時備份 Docker Volume 資料至雲端儲存空間 (S3 或 Dropbox)。
    *   **AI 協作點：** 讓 AI 寫出備份與清理舊資料的定時任務 (Cronjob)。
2.  **收費開通流程：**
    *   **初期：人工開通。** 業務收款後，技術人員手動在 FUXA 中建立帳號並分配設備。
    *   **計費模型建議：**
        *   基礎版：固定 10 個 Tag，資料保存 30 天。
        *   進階版：不限 Tag，資料保存 1 年 + 定期報表。
3.  **AI 輔助維護流程：**
    *   建立常見問題的 AI Prompt 庫，讓客服能利用 AI 協助客戶排解 Modbus 地址對接問題。

---

## 預期成果
*   **增加現有產品銷售：** 客戶購買 I/O 模組時，可直接加購「雲端帳號」，大幅增加購買慾望。
*   **提升競爭力：** 提供與泓格等一線大廠同等級的「雲端可視化」能力，但成本更低。

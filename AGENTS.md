# 給 AI（Codex、Claude 等）的工作規則

這是 Yui 的減重紀錄 App「小仙女養成計畫」。Yui 是程式初學者，所有程式都由 AI 撰寫。

## 開始改之前
1. 先讀 `STYLE.md`（版面風格）、`COACH.md`（教練貼上格式）。
2. 整個 App 只有一個檔案：`index.html`（圖示檔 `icon.svg`、`icon-192.png`、`icon-512.png`、`apple-touch-icon.png`、`manifest.webmanifest` 另外放；主畫面圖示是 Yui 用 GPT 畫的鬱金香；瀏覽器分頁小圖示 `icon.svg` 是 🌷 emoji）。**不要拆成多個檔案、不要改用框架（React、Vue、Tailwind 等）**。
3. 交付時給 Yui「完整的檔案」，不要只給片段或 diff。

## 一定要保留的東西
- **版面風格**：奶茶＋莫蘭迪色、宋體標題、圓角卡片、底部 4 個分頁（今天／紀錄／猛健樂／設定）。顏色只能用 `:root` 裡的變數，不要寫死新顏色。Yui 很喜歡現在的樣子，**不要重新設計**。
- **「📋 貼上教練估算」**：解析規則在 `index.html` 第 8 節 `parsePaste()`，格式寫在 `COACH.md`。改格式時兩邊要一起改。
- **「複製今天的日誌」**：輸出格式要和 Yui 的 iOS 日誌 App 一模一樣（`journalText()`），不要改字、不要改 emoji。
- **體重紀錄收合**：預設顯示最近 5 筆，「展開全部（共 N 筆）▼」／「收起 ▲」。
- **編輯功能**：點餐點、體重、施打紀錄、生活紀錄都可以修改；刪除要按兩次。
- **設定頁「給教練的說明」**（`COACH_TEXT()`）：和 `COACH.md` 內容一致。
- 有膚色的 emoji 一律用最淺膚色（🏻）。

## 資料
- Firebase 專案：`home-hub-be41f`（和嘟嘟一家共用）。
- 這個 App 的資料只放在 `dietlog_users/yui/` 底下（Yui 的兩個 Gmail 共用），**不要讀寫其他 collection**（那是嘟嘟一家的資料）。
- 能登入的 Gmail 寫在 Firebase 後台的安全規則裡；repo 的 `firestore.rules` 只放空白，**不要把 Gmail 地址寫進 repo**（repo 是公開的）。
- 資料結構見 `README.md`「資料結構」。新增欄位要相容舊資料（舊欄位 `water`、`exercise`、`sideEffect` 是文字）。

## 改完要檢查
- 手機寬度 375px 看起來正常，字不會擠出卡片。
- 貼上 `COACH.md` 裡的範例，每一行都能正確加入。
- 「複製今天的日誌」的文字格式沒有變。

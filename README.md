# 小仙女養成計畫｜減重紀錄 App

手機優先的減重紀錄 App。單一 HTML 檔，放在 GitHub Pages，資料存在 Firebase（用 Google 登入，手機和電腦同步）。

- 網址（開好 GitHub Pages 後）：https://yamap610.github.io/diet-log/
- 給 AI 的規則：`AGENTS.md`
- 版面風格：`STYLE.md`
- 教練貼上格式：`COACH.md`

## 功能

| 分頁 | 內容 |
|---|---|
| 📝 今天 | 日誌卡（猛健樂劑次、體重）、餐點（📋 貼上教練估算／自己輸入／點一下修改）、熱量蛋白質進度、喝水杯（點一下＋1 瓶）、運動、心情、副作用（自動顯示打針後第幾天）、生活紀錄、教練的話、複製今天的日誌 |
| 📈 紀錄 | 體重統計＋曲線、體重紀錄（最近 5 筆，可展開／收起，點一下修改）、月曆（顏色＝熱量達標／超過／吃太少）、每週總覽 |
| 💉 猛健樂 | 下一劑倒數、這一針打了幾劑、部位輪替提醒、施打紀錄（可新增修改）、打針後第幾天最不舒服 |
| ⚙️ 設定 | 帳號、目標、給教練的說明（一鍵複製）、下載備份、匯入資料 |

## 第一次設定（只要做一次）

### 1. 開 GitHub Pages
repo → Settings → Pages → Source 選「Deploy from a branch」→ Branch 選 `main`、資料夾 `/ (root)` → Save。
等 1–2 分鐘，就能打開 https://yamap610.github.io/diet-log/

### 2. Firebase 開啟 Google 登入
1. 到 https://console.firebase.google.com/ → 選 `home-hub-be41f`
2. 左邊「Authentication」→ 如果是第一次，按「開始使用」
3. 「Sign-in method」→「Google」→ 啟用 → 選你的 Email 當支援信箱 → 儲存
4. 「Settings（設定）」→「Authorized domains（授權網域）」→「Add domain」→ 輸入 `yamap610.github.io` → 新增

### 3. Firebase 安全規則
1. 左邊「Firestore Database」→ 上面「規則（Rules）」
2. **先把原本的規則整段複製起來存好**（萬一要改回來）
3. 把 `firestore.rules` 的內容整段貼上，**把「填第一個Gmail」「填第二個Gmail」換成你的兩個 Gmail**，按「發布」
   （repo 是公開的，所以檔案裡只放空白，真正的 Gmail 只填在 Firebase 後台）

> 為什麼要改：這個 App 的資料只有你的兩個 Gmail 登入後能看，兩個帳號看到的是同一份紀錄。
> 嘟嘟一家原本的資料（items、diary_entries…）維持原本的設定，不會壞掉。
> 如果原本的規則裡有 `match /{document=**}` 開放全部，那條要拿掉，不然減重資料也會被公開。

### 4. 把舊網頁的紀錄搬過來
1. 打開舊的 claude.ai 飲食紀錄網頁 →「月曆」→「下載全部紀錄」→「💾 完整備份檔」
2. 打開這個 App → 用 Google 登入 →「設定」→「📥 匯入資料」→ 選剛剛下載的檔案 →「匯入」

### 5. 加到手機主畫面
iPhone Safari 打開網址 → 分享 →「加入主畫面」。

## 資料結構（Firestore）

全部放在 `dietlog_users/yui/` 底下（兩個 Gmail 共用；能登入的 Gmail 寫在 Firebase 安全規則）：

```
dietlog_users/yui              { settings: { kcalGoal, proteinGoal, waterGoal, bottleMl, startWeight, goalWeight, finalGoalWeight, startDate } }
dietlog_users/yui/days/{YYYY-MM-DD}
    meals: [{ type: 早餐|午餐|點心|晚餐, name, kcal, protein, tip, status: "done" }]
    waterMl: 1920
    exercises: [{ type: "🏃🏻‍♀️ 超慢跑", min: 15 }]
    mood: "🙂"
    sideEffects: { items: ["🎈 脹氣"], level: 輕|中|重, note }
    steps, bm, supplement, sleep: 文字
    notes: ["教練的話"]
    （舊網頁匯入的欄位：water、exercise、sideEffect 為文字，App 仍會顯示）
dietlog_users/yui/weights/{YYYY-MM-DD}   { kg, note }
dietlog_users/yui/shots/{YYYY-MM-DD}     { dose, site, note, pen, penDose }
```

備份檔（JSON）格式：`{ app, exportedAt, settings, days: [{id, ...}], weights: [{id, ...}], shots: [{id, ...}] }`，和舊的 claude.ai 網頁備份檔相容。

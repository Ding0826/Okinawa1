# 旅遊小工具 Travel Web App

單一 HTML 檔案旅遊管理工具，無需後端，所有資料儲存於瀏覽器 LocalStorage。

---

## 快速啟動

```bash
cd D:\new_work\travel
python -m http.server 5500
```

開啟瀏覽器：`http://localhost:5500/index.html`

> ⚠️ 不可直接用 `file://` 開啟，需透過 HTTP Server。

---

## 功能頁籤

| 頁籤 | 功能 |
|---|---|
| **PLAN** | 每日行程時間軸、Google Maps 連結 |
| **GUIDE** | 景點深度介紹、OpenStreetMap 嵌入地圖 |
| **WALLET** | 計算機匯率換算、多幣別記帳、照片上傳 |
| **LISTS** | 隨身/托運清單打勾、個人備忘錄 |
| **INFO** | 天氣連結、緊急電話、出入境注意事項 |
| **DASH** | 今日行程、每日花費長條圖、清單進度、資料備份 |

---

## 資料備份

DASH 頁最下方 → **匯出 JSON** 可下載備份檔案  
換裝置或清除瀏覽器前請先備份，再用 **匯入備份** 還原。

---

## 發佈方式

- **Netlify Drop**：拖曳 `index.html` 至 [netlify.com/drop](https://netlify.com/drop)，30 秒取得公開網址
- **GitHub Pages**：push 至 repo，Settings → Pages 啟用
- **區網分享**：查詢本機 IP，手機連 `http://192.168.x.x:5500`
- **更新注意**：更新git之後 要再執行git push upstream main，才會推到私人git帳號


---

## 下次建立新旅遊版本的提示詞

```
# Role
你是一位精通 HTML5、Tailwind CSS（CDN 版）和 Vanilla JavaScript 的資深前端工程師。
請幫我製作一個「單頁式旅遊 Web App（Single File HTML）」，不需要後端，所有資料儲存於瀏覽器 LocalStorage。

---

# Goal
製作一個適合手機瀏覽、介面精美、功能實用的旅遊小工具，供親友在旅行時使用。

---

# Design Style（視覺風格）

主題色彩請使用 Tailwind `extend.colors` 自訂調色盤，以以下命名慣例定義：
```
tripId: {
  primary: '主色（深）',
  secondary: '主色（中）',
  accent: '強調色（珊瑚/暖色）',
  bg: '淺底色',
  mist: '極淺底色',
  teal: '對比色'
}
```
**[填入風格描述，例：沖繩海島、京都和風、首爾街頭潮流⋯]**

字體請從 Google Fonts 挑選三款：
- `display`：標題裝飾字（例：Zen Maru Gothic、Shippori Mincho）
- `body`：Noto Sans TC + Noto Sans JP（通用中日文）
- `number`：Inter（數字/時間）

Header 使用 135° 漸層背景，加入 SVG 波浪裝飾與 emoji 浮水印。
卡片使用圓角 16px、投影 `rgba(主色, 0.10)`；hover 輕微上浮。
按鈕分兩種：`btn-primary`（主色漸層）、`btn-accent`（強調色漸層）。

---

# Input Data（行程資訊）

## 1. 基本資訊
- 旅遊標題：[例：沖繩海島度假]
- 副標（英文 + 日期）：[例：Okinawa Escape　2026.5.28 - 2026.6.1]
- Header Emoji：[例：🐈]
- 裝飾 Emoji（右上角兩個）：[例：🌊 🐾]

## 2. 航班資訊
去程：[航空公司 + 班號，起飛時間 → 抵達時間，出發地 → 目的地，日期（星期）]
回程：[航空公司 + 班號，起飛時間 → 抵達時間，出發地 → 目的地，日期（星期）]

## 3. 住宿資訊
[每晚一筆，格式：D幾 & D幾：飯店名稱，入住日期]
例：
D1 & D4：那霸休伊特渡假飯店，5/28, 5/31
D2 & D3：Airbnb 讀谷公寓，5/29, 5/30

## 4. 每日行程（PLAN_DATA）
每天一個物件，格式如下：
{
  date: 'M/DD (週)',
  day: 'D幾',
  theme: '當日主題描述',
  hotel: '當晚住宿名稱',
  items: [
    { time: 'HH:MM', icon: 'emoji', title: '行程名稱', desc: '簡短說明',
      color: 'bg-xxx（Tailwind 色碼）',
      alert: true/false（重要時間點加警示標籤）,
      mapLink: 'Google Maps URL（選填）',
      mapLabel: '按鈕文字（選填）' }
  ]
}
[依天數填入所有天]

## 5. 深度導覽（GUIDE_DATA）
每個景點一個物件：
{
  title: '景點名稱',
  subtitle: '英文或日文副標',
  icon: 'emoji',
  tags: ['標籤1', '標籤2'],
  intro: '3～5 句深度介紹文字',
  facts: ['冷知識1', '冷知識2', '冷知識3'],
  mapQuery: 'Google Maps 搜尋關鍵字',
  mapEmbed: 'OpenStreetMap embed URL（bbox 參數請對應景點座標）'
}
[填入此行程的重點景點，建議 5～10 個]

## 6. 行前清單
隨身行李（CARRY_ITEMS）：[逐項填入，例：護照、日幣/台幣、行動電源、網卡、耳機⋯]
托運行李（CHECK_ITEMS）：[逐項填入，例：換洗衣物、充電器、盥洗用具、個人藥品⋯]

## 7. 當地貨幣設定
幣別符號：[例：¥ / ₩ / ฿]
幣別代碼：[例：JPY / KRW / THB]
預設匯率：100 當地貨幣 = ? NT$（例：¥100 ≈ NT$23）
LocalStorage 前綴（英數，無空格）：[例：oki2026 / seoul2026]

## 8. 旅遊日期區間（用於 Countdown 與今日行程對應）
出發日期時間（ISO 格式含時區）：[例：2026-05-28T16:15:00+08:00]
回程結束日期（ISO 格式）：[例：2026-06-01T23:59:00+08:00]
花費記錄日期下拉選項：[例：5/28、5/29、5/30、5/31、6/1]

---

# Functional Requirements（功能模組）

底部固定 6 個導航按鈕（FontAwesome 圖示 + 文字），切換以下分頁：

### 1. 總覽（dashboard）
- Header 右上角顯示：倒數天數 / 旅途中 / 已結束（依系統時間自動判斷）
- Header 右下角顯示：今日花費（從 LocalStorage 動態計算）
- 航班速查卡片（去程 / 回程各一）
- 住宿一覽卡片（清單式，每晚一列）
- 花費統計：總花費、今日花費、筆數（雙幣顯示）
- Chart.js 4 每日花費長條圖（Canvas）
- JSON 匯出 / 匯入備份按鈕
- 快速連結格子（天氣、地圖、警察、救護）

### 2. 行程（plan）
- 頂部日期切籤（水平捲動），自動對應今日
- 時間軸卡片：時間 → emoji → 標題 → 描述，重要項目加左側 coral 邊框
- Google Maps 按鈕（rounded-full 小按鈕）

### 3. 導覽（guide）
- 景點卡片：emoji 大圖示、標題、副標、標籤 badge
- 深度介紹段落 + 冷知識區塊
- Google Maps 導航按鈕
- OpenStreetMap iframe 嵌入地圖（高度 180px）

### 4. 記帳（wallet）
- 匯率設定（100 當地幣 = ? NT$）
- 快速換算計算機（支援算式 500+200*3）
- 新增花費表單：日期下拉、類別下拉、品項說明、幣別切換、金額、收據照片上傳
  - 幣別 TWD 時自動換算存入當地幣
  - 照片用 Canvas 壓縮（600px, quality 0.7）存為 Base64
- 花費明細列表：日期篩選、總計顯示、刪除按鈕、點圖放大 Modal
- CSV 匯出按鈕

### 5. 清單（lists）
- 隨身行李 / 托運行李 CheckList（打勾持久化，顯示完成進度）
- 個人備忘錄（textarea，URL 自動轉連結，清除按鈕）

### 6. 資訊（info）
- 天氣查詢連結（目的地氣象廳 / Weather.com）
- 當季氣候提示
- 緊急聯絡電話（可撥打 href="tel:"）：警察、消防救護、台灣駐外辦事處、外交部急難救助
- 目的地旅遊注意事項（5～6 條，各用不同色系卡片）
- 出入境違禁品提醒（3～4 條）

---

# Technical Constraints（技術規格）

- **Single File**：全部 CSS、JS 內嵌於同一 HTML 檔案
- **RWD**：Mobile-First，`max-width: 672px` 置中，`html { font-size: 17px }`
- **Libraries（全部使用 CDN）**：
  - Tailwind CSS CDN（含 `tailwind.config` 自訂色彩）
  - FontAwesome 6.5
  - Chart.js 4.4.0（`chart.umd.min.js`）
  - Google Fonts（display / body / number 三款）
- **No Backend**：純前端
- **LocalStorage Key 命名**：`{前綴}_expenses`、`{前綴}_checks`、`{前綴}_memo`、`{前綴}_rate`
- **匯率儲存單位**：100 當地幣 = X TWD（變數命名 `ratePerHundred`）
- **CSS 細節**：
  - 自訂 scrollbar（4px，主色）
  - `.page-section { display: none; }` / `.page-section.active { display: block; }`
  - `.card:hover { transform: translateY(-2px); }`
  - `@keyframes fadeIn` 切換動畫

請輸出完整可直接執行的 HTML 程式碼。
```

---

## 風格替換建議

| 目的地 | 風格關鍵字 |
|---|---|
| 沖繩 | 海灘風格，搭配吉卜力龍貓設計元素 |
| 京都 | 和風禪意，楓葉與金閣寺配色 |
| 首爾 | K-Pop 街頭潮流風，霓虹色系 |
| 峇里島 | 熱帶度假風，棕櫚葉與珊瑚色 |
| 北海道 | 清新雪景風，薰衣草紫與冰藍色 |
| 東京 | 現代都市風，霓虹夜景配色 |

---

## 貨幣代碼對照

| 國家 | 代碼 | 參考匯率（TWD） |
|---|---|---|
| 日本 | JPY | ≈ 0.218 |
| 韓國 | KRW | ≈ 0.023 |
| 泰國 | THB | ≈ 0.9 |
| 歐洲 | EUR | ≈ 32 |
| 美國 | USD | ≈ 30 |

# kyushu-trip-2026

福岡・熊本 8 天 7 夜自由行 App（2026/12/7–14）。
單一 HTML 檔，部署在 GitHub Pages，用 Firebase Firestore 讓全團即時同步，可加入 iPhone 主畫面當 App 使用。

**網址**：https://david810213-boop.github.io/kyushu-trip-2026/

## 功能

### 行程
- 8 天景點導覽：上一站／下一站、進度條、當日景點清單
- 景點可新增、編輯、刪除、移到其他天，依時間自動排序（全團同步）
- 一鍵開 Google 地圖、複製地址

### 航班退稅
- 星宇航空去回程航班資訊
- 回程熊本機場 Lounge ASO：動線示意圖、使用步驟、JCB 卡使用條件
- 熊本機場退稅攻略（2026/11/1 退稅新制）、檢查清單（每人各自勾選，存在本機）

### 記帳
- 多人共用記帳：付款人、分攤成員、日圓換台幣
- 自動結算誰該給誰多少；匯率全團同步

### 美食購物
- **美食推薦**：食べログ評分、每人價位、預約連結，分類手繪插圖篩選（含爐端燒）
- **伴手禮**：推薦清單
- **購物・刷卡・退稅**：
  - 購物優惠券（唐吉訶德、Bic Camera、藥妝店、百貨），附各店最推薦刷哪張卡
  - 12 月刷卡攻略：持有卡片回饋排行、Mastercard Travel Rewards／JCB MyJapan+ 登錄教學
  - 各店退稅手續費比較
- 口袋美食可自行新增（價位、預約連結），收藏全團同步

### 成員・留言
- 旅伴留言板：便利貼、5 種顏色、置頂、刪除；有新留言時底部分頁顯示紅點
- 護照資料、Visit Japan Web QR Code（離線可看、可全螢幕給機台掃描）
- 緊急聯絡電話

### 其他
- 字體大小切換：標準／大／特大，每支手機各自記住（預設「大」）
- 離線可用：資料快取在手機上，離線時的修改會在連網後自動上傳

## 多人同步怎麼用
1. 一位旅伴打開網頁，按「建立新的行程房間」
2. 按標題列的「分享連結」傳給其他人，打開連結就會自動加入
3. 房號就是鑰匙，只分享給旅伴
4. 用 Safari 開啟後「加入主畫面」，之後從主畫面開即可；更新後不需重新加入

## 資料說明
- 美食評分與價位：食べログ（2026/11 查詢）
- 優惠券、刷卡回饋、退稅手續費：2026/11/10 查證，適用 12/7–14；各店與銀行可能調整，出發前請再確認
- 程式內建的成員名單只用於「新建房間」；已建立房間的成員請在 App 成員頁編輯

## 技術架構
- 單一 `index.html`：Tailwind CSS（CDN）、Font Awesome、原生 JavaScript（ES Module）
- Firebase：Authentication（匿名登入）＋ Firestore（`persistentLocalCache` 離線快取）
- 部署：GitHub Pages

### Firestore 資料結構
```
rooms/{roomId}                    { exchangeRate, spotsSeeded, createdAt, createdBy }
rooms/{roomId}/access/{uid}       加入房間的登記（安全規則用）
rooms/{roomId}/members/{mid}      { name, passport, expiry, order }
rooms/{roomId}/qr/{mid}           { img (PNG data URL), updatedAt }
rooms/{roomId}/spots/{id}         { day, time, name, tag, desc, addr, svg, order, createdAtMs }
rooms/{roomId}/expenses/{id}      { item, amount, payerId, splitterIds[], createdAtMs, createdBy }
rooms/{roomId}/foods/{id}         { type, name, tag, desc, location, price, reserveUrl, createdAtMs }
rooms/{roomId}/favorites/{foodId} { fav }
rooms/{roomId}/notes/{id}         { text, color, authorId, authorName, pinned, createdAtMs, createdBy }
```

### 存在本機（localStorage）的設定
- 目前房號、退稅檢查清單勾選、字體大小、留言身分、已讀留言時間

## Firebase 設定
- Authentication → 登入方式：啟用「匿名」
- Firestore Database：地區 `asia-east1`
- 安全規則：把 `firestore.rules` 內容貼到 Console → Firestore Database → 規則 → 發布
  （只有加入房間的人能讀寫該房間資料，且不能列出其他房間）

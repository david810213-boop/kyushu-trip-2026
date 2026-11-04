# kyushu-trip-2026
自由行用APP（單一 HTML，GitHub Pages 部署，Firebase Firestore 多人即時同步）

## 功能
- 行程：8 天景點導覽、Google 地圖、複製地址
- 航班退稅：熊本機場 2026/11 新制攻略、檢查清單（每人各自勾選，存本機）
- 記帳：多人共用記帳、分攤成員、自動結算、匯率（全團同步）
- 美食：推薦清單、口袋名單、收藏（全團同步）
- 成員：護照資料、Visit Japan Web QR（全團同步，離線可看，可全螢幕顯示）

## 多人同步
1. 一位旅伴打開網頁 →「建立新的行程房間」
2. 按標題列的「分享連結」傳給其他人，打開連結即自動加入
3. 房號就是鑰匙，只分享給旅伴

## Firestore 資料結構
```
rooms/{roomId}                    { exchangeRate, createdAt, createdBy }
rooms/{roomId}/access/{uid}       加入房間的登記（安全規則用）
rooms/{roomId}/members/{mid}      { name, passport, expiry, order }
rooms/{roomId}/qr/{mid}           { img (PNG data URL), updatedAt }
rooms/{roomId}/expenses/{id}      { item, amount, payerId, splitterIds[], createdAtMs, createdBy }
rooms/{roomId}/foods/{id}         { type, name, tag, desc, location, ..., createdAtMs }
rooms/{roomId}/favorites/{foodId} { fav }
```

## Firebase 設定
- Authentication：啟用「匿名」登入
- Firestore：把 `firestore.rules` 內容貼到 Console → Firestore Database → 規則 → 發布

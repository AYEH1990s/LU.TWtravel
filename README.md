# 🗺 Taiwan Travel Guide — 部署教學

## 📁 檔案說明

| 檔案 | 說明 |
|------|------|
| `index.html` | 前台網頁（外國友人瀏覽） |
| `admin.html` | 後台管理介面（需登入） |
| `firebase-config.js` | Firebase 設定檔（你填入金鑰） |
| `README.md` | 本教學文件 |

---

## STEP 1｜申請 Firebase 帳號並建立專案

1. 前往 [https://console.firebase.google.com](https://console.firebase.google.com)
2. 用 Google 帳號登入
3. 點「**建立專案**」
4. 專案名稱輸入：`taiwan-travel`（或任意名稱）
5. 關閉 Google Analytics（不需要）→ 點「**建立專案**」
6. 等待建立完成，點「**繼續**」

---

## STEP 2｜取得 Firebase 設定金鑰

1. 在 Firebase 控制台，點左側「**專案設定**」（齒輪圖示）
2. 往下滾到「**您的應用程式**」區塊
3. 點「**</>**」（網頁應用程式圖示）
4. 應用程式暱稱輸入：`taiwan-travel-web` → 點「**註冊應用程式**」
5. 複製畫面上的設定物件，格式如下：

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "taiwan-travel-xxxxx.firebaseapp.com",
  projectId: "taiwan-travel-xxxxx",
  storageBucket: "taiwan-travel-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

6. 打開 `firebase-config.js`，將每個值貼入對應欄位
7. `databaseURL` 請填入：`https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com`
   （將 YOUR_PROJECT_ID 換成你的專案 ID）

---

## STEP 3｜啟用 Realtime Database

1. Firebase 控制台 → 左側「**建構**」→「**Realtime Database**」
2. 點「**建立資料庫**」
3. 地區選「**us-central1**」（預設即可）→「**下一步**」
4. 選「**以測試模式開始**」→「**啟用**」
5. 啟用後，點上方「**規則**」分頁
6. 將規則改為以下內容（保護資料安全）：

```json
{
  "rules": {
    "places": {
      ".read": true,
      ".write": "auth != null"
    }
  }
}
```

7. 點「**發佈**」

> 說明：`".read": true` 讓前台可以讀取景點資料；
> `".write": "auth != null"` 只有登入的管理員才能新增/修改/刪除。

---

## STEP 4｜建立管理員帳號

1. Firebase 控制台 → 左側「**建構**」→「**Authentication**」
2. 點「**開始使用**」
3. 點「**電子郵件/密碼**」→ 開啟第一個選項 → 「**儲存**」
4. 點「**使用者**」分頁 → 「**新增使用者**」
5. 輸入管理員 Email 和密碼 → 點「**新增使用者**」
6. 重複步驟 5 為每位協作者建立帳號

> 安全提示：密碼請使用 12 字元以上，含大小寫和數字。

---

## STEP 5｜上傳到 GitHub Pages

1. 前往 [https://github.com](https://github.com) 登入
2. 建立新 Repository，名稱例如：`taiwan-travel`
3. 勾選「**Public**」（GitHub Pages 免費版需要 Public）
4. 建立後，上傳以下 4 個檔案：
   - `index.html`
   - `admin.html`
   - `firebase-config.js`
   - `README.md`
5. 上傳方式：點「**Add file**」→「**Upload files**」→ 拖曳上傳

### 開啟 GitHub Pages
1. 進入 Repository → 點上方「**Settings**」
2. 左側點「**Pages**」
3. Source 選「**Deploy from a branch**」
4. Branch 選「**main**」→ 資料夾選「**/ (root)**」
5. 點「**Save**」
6. 等待約 1 分鐘，頁面會出現你的網址：
   `https://你的帳號.github.io/taiwan-travel/`

---

## STEP 6｜測試是否正常運作

1. 前台：`https://你的帳號.github.io/taiwan-travel/`
   - 應顯示空白景點列表（尚未新增資料）

2. 後台：`https://你的帳號.github.io/taiwan-travel/admin.html`
   - 應看到登入畫面
   - 輸入 Step 4 建立的帳號密碼
   - 登入後點「Add Place」新增第一筆景點
   - 回到前台重新整理，應看到景點出現

---

## 日常使用流程

### 新增景點
1. 前往 `你的網址/admin.html`
2. 登入
3. 點「Add Place」填入資訊 → 儲存
4. 前台自動更新（不需要重新部署）

### 邀請協作者
1. Firebase 控制台 → Authentication → 新增使用者
2. 把 `admin.html` 網址和帳號密碼傳給協作者

### 修改景點
1. 登入後台 → 找到景點 → 點「Edit」

### 刪除景點
1. 登入後台 → 找到景點 → 點「Delete」

---

## 常見問題

**Q: 前台顯示「Unable to load data」？**
A: 檢查 `firebase-config.js` 的設定值是否正確，特別是 `databaseURL`。

**Q: 登入後台失敗？**
A: 確認 Firebase Authentication 已開啟「電子郵件/密碼」登入方式。

**Q: 修改後前台沒有更新？**
A: Firebase Realtime Database 是即時同步的，重新整理前台應立即看到變更。

**Q: 要加新管理員怎麼做？**
A: Firebase 控制台 → Authentication → 新增使用者。

---

## 聯絡技術支援

如有問題，請將錯誤訊息截圖，回到 Claude 對話中描述問題。

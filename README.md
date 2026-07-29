# Food Tracking

離線優先的飲食日記：餐點、熱量、巨量營養素、飲水、運動、步數、體重。Expo + SQLite + Supabase。介面支援英文與繁體中文。

> **狀態：** 開發中。**iOS** 為主要平台；Android 可能可用；**Web 僅供預覽**。

## 快速開始

所有指令請在 `food-tracking/` 目錄執行。

```sh
git clone https://github.com/jacky620141760/Food-Tracking.git
cd Food-Tracking/food-tracking
npm ci
```

Windows 複製環境範本：

```powershell
Copy-Item .env.example .env
```

macOS / Linux：

```sh
cp .env.example .env
```

接著依下方 **[連接 database](#連接-database)** 填入 `.env`，然後：

```sh
npm start              # 手機與電腦同一 Wi‑Fi：用 Expo Go 掃 QR
npm run start:tunnel   # 不同網路時用 tunnel
```

**需要：** Node.js 22+、iPhone + [Expo Go](https://expo.dev/go)（或 Xcode Simulator）。登入、雲端同步、條碼查詢需設定 Database。

---

## 連接 Database

每位開發者／使用者需**用自己的 Database，在本地 `.env` 連線。



### 1. 取得 API 金鑰並寫入 `.env`

1. - **URL** → `EXPO_PUBLIC_SUPABASE_URL`
   - **key** → `EXPO_PUBLIC_SUPABASE_ANON_KEY`

2. 重新啟動 Expo（`Ctrl+C` 後再 `npm start`）。

### 2. 驗證連線

1. 用手機 Expo Go 開啟 App。
2. 用 email **註冊／登入**（不要用「臨時訪客」若你要保留資料）。
3. 設定減重計劃並儲存。
4. **更多 → 立即同步**，應顯示「已與雲端同步完成」。

## 指令

| 指令 | 用途 |
|------|------|
| `npm start` | 開發伺服器（區網） |
| `npm run start:tunnel` | Tunnel 模式 |
| `npm run ios` / `android` / `web` | 開啟對應平台 |
| `npm run check` | Lint、型別檢查、測試 |

## 目錄

```text
food-tracking/
├── app/           Expo Router 畫面
├── src/           UI、domain、SQLite、services
├── supabase/      schema.sql、Edge Functions
├── .env.example   環境變數範本（可提交）
└── package.json
```

產品規格：[`food-tracking/requirements.md`](food-tracking/requirements.md)

## 貢獻與授權

[CONTRIBUTING.md](CONTRIBUTING.md) · [SECURITY.md](SECURITY.md) · [MIT License](LICENSE)

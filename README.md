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

接著依下方 **[連接 Supabase](#連接-supabase)** 填入 `.env`，然後：

```sh
npm start              # 手機與電腦同一 Wi‑Fi：用 Expo Go 掃 QR
npm run start:tunnel   # 不同網路時用 tunnel
```

**需要：** Node.js 22+、iPhone + [Expo Go](https://expo.dev/go)（或 Xcode Simulator）。登入、雲端同步、條碼查詢需設定 Supabase。

---

## 哪些不會上傳到 Git？

以下檔案**只存在你的電腦**，已在 `.gitignore` 排除，`git push` **不會**帶上去：

| 類型 | 路徑 / 範例 | 說明 |
|------|-------------|------|
| API 金鑰與設定 | `food-tracking/.env` | Supabase URL、anon key 等 |
| VM / Expo token | `food-tracking/.env.vm` | 遠端 tunnel 用，非一般開發 |
| 本機資料庫 | `haelf_nutrition.db` | App 內 SQLite，存在手機／模擬器上 |
| 依賴與建置 | `node_modules/`、`.expo/`、`dist/` | 可本地重建 |

**可以提交的是** 範本檔（不含真實金鑰）：

- `food-tracking/.env.example`
- `food-tracking/.env.vm.example`

**切勿提交：** service-role key、資料庫密碼、AI API key、`.env` 實檔、任何 `*.db`。

推送前可自查：

```sh
git status
```

若看到 `.env` 或 `.db` 出現在待提交列表，**不要 commit**，先確認 `.gitignore` 是否生效。

---

## 連接 Supabase

每位開發者／使用者需**用自己的 Supabase 專案**（或團隊共用專案），在本地 `.env` 連線。

### 1. 建立 Supabase 專案

1. 前往 [supabase.com](https://supabase.com) 登入並 **New project**。
2. 記下專案名稱與 Database 密碼（僅 Supabase 後台用，**不要**寫進 App）。

### 2. 執行資料庫 Schema

1. 在 Supabase 左側開 **SQL Editor** → **New query**。
2. 開啟本 repo 的 [`food-tracking/supabase/schema.sql`](food-tracking/supabase/schema.sql)，全選複製貼上。
3. 按 **Run**。

這會建立同步用的資料表（含 `app_preferences` 的減重計劃欄位、`daily_goal_versions` 等）與 RLS 政策。

> 若專案是早期建立的，`app_preferences` 可能缺少減重欄位。請再執行 schema.sql 裡的 `ALTER TABLE ... add column if not exists` 段落，或重新跑完整 schema。

### 3. 取得 API 金鑰並寫入 `.env`

1. Supabase → **Project Settings**（齒輪）→ **API**。
2. 複製：
   - **Project URL** → `EXPO_PUBLIC_SUPABASE_URL`
   - **anon public** key → `EXPO_PUBLIC_SUPABASE_ANON_KEY`
3. 編輯 `food-tracking/.env`：

```dotenv
EXPO_PUBLIC_SUPABASE_URL=https://xxxxxxxx.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJI...
```

4. 重新啟動 Expo（`Ctrl+C` 後再 `npm start`）。

### 4. 驗證連線

1. 用手機 Expo Go 開啟 App。
2. 用 email **註冊／登入**（不要用「臨時訪客」若你要保留資料）。
3. 設定減重計劃並儲存。
4. **更多 → 立即同步**，應顯示「已與雲端同步完成」。
5. 在 Supabase **Table Editor** 檢查你的 `user_id` 下是否有 `daily_goal_versions`、`app_preferences` 資料。

### 5. 選用：條碼搜尋 Edge Function

```sh
cd food-tracking
supabase login
supabase link --project-ref <你的-project-ref>
supabase functions deploy off-search --no-verify-jwt
```

`<project-ref>` 在 Supabase 專案 URL 中：`https://supabase.com/dashboard/project/<project-ref>`。

---

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

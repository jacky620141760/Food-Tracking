# Food Tracking

Offline-first nutrition diary: meals, calories, macros, water, exercise, steps, and weight. Built with Expo, SQLite, and Supabase. English and Traditional Chinese.

> **Status:** Active development. **iOS** is the primary target. Android may work; **web is preview only**.

## Quick start

All app commands run from `food-tracking/`.

```sh
git clone <repository-url>
cd <repository-directory>/food-tracking
npm ci
cp .env.example .env   # Windows: Copy-Item .env.example .env
```

Set in `.env`:

```dotenv
EXPO_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=YOUR_ANON_KEY
```

```sh
npm start              # same Wi‑Fi: scan QR in Expo Go
npm run start:tunnel   # different network (needs ngrok)
```

**Requires:** Node.js 22+, iOS device with [Expo Go](https://expo.dev/go) (or Xcode Simulator). Supabase is required for sign-in, sync, and barcode lookup.

## Supabase

1. Create a project at [supabase.com](https://supabase.com).
2. In **SQL Editor**, run [`food-tracking/supabase/schema.sql`](food-tracking/supabase/schema.sql) (includes weight-plan columns on `app_preferences`).
3. Copy project URL and anon key into `food-tracking/.env`.
4. Optional — barcode search Edge Function:

   ```sh
   cd food-tracking
   supabase login
   supabase link --project-ref <project-ref>
   supabase functions deploy off-search --no-verify-jwt
   ```

Never commit `.env` or put service-role keys in `EXPO_PUBLIC_*` variables.

## Commands

| Command | Purpose |
| --- | --- |
| `npm start` | Dev server (LAN) |
| `npm run start:tunnel` | Dev server via tunnel |
| `npm run ios` / `android` / `web` | Open platform |
| `npm run check` | Lint, typecheck, tests |

## Layout

```text
food-tracking/
├── app/           Expo Router screens
├── src/           UI, domain logic, SQLite, services
├── supabase/      schema.sql, Edge Functions
├── .env.example
└── package.json
```

Product details: [`food-tracking/requirements.md`](food-tracking/requirements.md). Agent notes: [`food-tracking/README.md`](food-tracking/README.md).

## Contributing & license

[CONTRIBUTING.md](CONTRIBUTING.md) · [SECURITY.md](SECURITY.md) · [MIT License](LICENSE)

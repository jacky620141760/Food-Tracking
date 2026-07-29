# Food Tracking

Food Tracking is an open-source, local-first nutrition and health diary built with Expo and React Native. Track meals, calories, macronutrients, water, exercise, steps, weight, and progress without creating an account or running a cloud database.

> **Status:** Under active development. iOS is the primary platform, Android may work, and web is intended for development preview only.

## Local-first by default

- App data is stored on the device in SQLite (`haelf_nutrition.db`).
- Local features work without a Supabase project, API keys, or an account.
- Schema migrations run on the device and preserve supported existing data.
- Cloud authentication and synchronization are optional integrations, not requirements.
- English and Traditional Chinese interfaces are included.

Network access is still required for uncached Open Food Facts lookups, optional AI analysis, and optional cloud synchronization.

## Quick start

**Requirements:** Node.js 22+, npm, and either Expo Go on a device or a supported simulator.

```sh
git clone https://github.com/jacky620141760/Food-Tracking.git
cd Food-Tracking/food-tracking
npm ci
npm start
```

Open the project with Expo Go or a simulator. If no cloud environment variables are configured, the app starts in local-only mode and stores data in on-device SQLite.

Useful alternatives:

```sh
npm run start:tunnel   # Use Expo through a tunnel
npm run ios            # Open the iOS target
npm run android        # Open the Android target
npm run web            # Development preview only
```

## Optional online database

For personal use, **Supabase is the recommended online database** because this repository already includes Supabase email authentication, synchronization, a Postgres schema, and row-level security (RLS) policies. It is optional: local-only users can ignore `.env.example` and the entire `supabase/` directory.

### Set up hosted Supabase

1. Create a project from the [Supabase Dashboard](https://supabase.com/dashboard). A separate project for your personal installation is recommended.
2. Open the project's **SQL Editor**, create a new query, copy the complete contents of [`food-tracking/supabase/schema.sql`](food-tracking/supabase/schema.sql), and run it. This creates the synchronization tables and owner-only RLS policies.
3. In **Authentication**, keep email/password sign-up enabled while creating your account. For a private installation, you can disable new sign-ups after your personal account has been created.
4. Open **Project Settings → API** (or **API Keys**) and copy:
   - the project URL into `EXPO_PUBLIC_SUPABASE_URL`;
   - the client-side **publishable key** or legacy **anon key** into `EXPO_PUBLIC_SUPABASE_ANON_KEY`.

Read the official [Supabase API key documentation](https://supabase.com/docs/guides/getting-started/api-keys) if your dashboard shows the newer publishable/secret key format.

### Configure the app

From `food-tracking/`, create a local environment file:

```powershell
# Windows PowerShell
Copy-Item .env.example .env
```

```sh
# macOS or Linux
cp .env.example .env
```

Edit `.env`:

```dotenv
EXPO_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT_REF.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=YOUR_PUBLISHABLE_OR_ANON_KEY
```

Restart Expo after changing the file:

```sh
npm start
```

When these values are valid, the app enables the account screen. Register or sign in with email and password, complete onboarding, add a test entry, and use the manual sync action in the app. Confirm the row appears in the corresponding Supabase table, then edit it on the device and sync again.

### Optional Open Food Facts search proxy

Barcode lookup can call Open Food Facts directly, so this Edge Function is not required for the core app. To enable the included text-search proxy, deploy [`food-tracking/supabase/functions/off-search`](food-tracking/supabase/functions/off-search) with the [Supabase CLI and Edge Functions guide](https://supabase.com/docs/guides/functions/quickstart). The repository's `supabase/config.toml` configures this function with JWT verification disabled.

### Security and data warnings

- **Never put a secret key, `service_role` key, or database password in an `EXPO_PUBLIC_*` variable.** Expo embeds these values in the client application. Use only a publishable or legacy anon key.
- Keep the RLS policies from `schema.sql` enabled. They restrict each signed-in user to rows whose `user_id` matches their authenticated account.
- Do not commit `.env`. It is local configuration even though the publishable/anon key is designed for client-side use.
- Switching the app to a different Supabase account clears locally stored app tables before pulling that account's cloud data. Do not switch accounts if the current local data has not synchronized successfully.
- Supabase availability, retention, quotas, and pricing are controlled by Supabase; review its current terms before relying on it as your only recovery copy.

The local SQLite repositories remain the primary application data layer. Supabase is the included optional adapter, not a requirement for forks; contributors may replace it with another remote backend while retaining the local repository interfaces.

The Supabase setup guidance above is paraphrased from the linked official documentation.

## Data and privacy

Local mode does not upload diary data for authentication or synchronization. Removing the app, clearing its storage, or losing the device may permanently remove local data; built-in export/import and guaranteed cross-device recovery are not currently provided. Device backup behavior depends on the operating system. Web storage and credential behavior do not represent the native iOS build.

## Validation

Run `npm run check` from `food-tracking/` to run linting, TypeScript checks, and the domain/repository verification scripts. Run `npm run export:web` for the additional web export check used by CI.

## Project structure

- `food-tracking/app/` — Expo Router screens
- `food-tracking/src/db/` — SQLite database, schema migrations, and local repositories
- `food-tracking/src/domain/` — domain types and nutrition logic
- `food-tracking/src/services/` — optional external services and synchronization
- `food-tracking/supabase/` — optional Supabase schema and Edge Functions

Contributions are welcome; see [CONTRIBUTING.md](CONTRIBUTING.md). Report vulnerabilities according to [docs/SECURITY_REVIEW.md](docs/SECURITY_REVIEW.md). This project is licensed under the MIT License.
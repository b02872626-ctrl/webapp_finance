# webapp_finance

Web companion for **Finance Lore** — Supabase-backed dashboard for balances,
transactions, category analysis, ledger (IOUs + recurring commitments),
income tracking with SMS auto-detection, and savings goals with priority
waterfall allocation. Now ships with Notion-style views (Cards · Table ·
Calendar / Progress) on the Ledger, Income, and Goals tabs.

## Run locally
```bash
npx --yes serve --listen 4321 .
open http://127.0.0.1:4321
```

## Deploy to Vercel (zero config)
1. https://vercel.com/new → Import this repo
2. Framework Preset: **Other** · Build Command: empty · Output Directory: empty
3. Deploy. No env vars needed — the Supabase anon key is gated by RLS and
   safe to ship in client code.

## Database setup
Apply SQL migrations in `supabase/` to your Supabase project in this order
when starting fresh (each is idempotent):

1. `secure_schema.sql`           — base tables + RLS
2. `add_category.sql`            — category column on transactions
3. `add_i_raw_data.sql`          — raw SMS storage
4. `add_recipt_link.sql`         — receipt URL column
5. `migrate_banks_to_catalog.sql`— bank-name normalisation
6. `add_ledger.sql`              — IOU + recurring commitments
7. `add_income.sql`              — income tracking + dismissal log
8. `add_custom_cadence.sql`      — bi-weekly / every-N-days cadence
9. `add_goals.sql`               — savings goals + contributions

Only `secure_schema`, `add_ledger`, `add_income`, `add_custom_cadence`, and
`add_goals` are strictly required for the web app — the rest support the
Android client.

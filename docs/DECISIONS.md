# Decisions

Short ADRs. Status is one of: Proposed, Accepted, Superseded.

## ADR-001: PGlite in the browser is the only metrics runtime

- **Status:** Accepted (2026-11-11), for visitor and logged-in modes.
- **Context:** Visitor mode must process the statement in the browser and save nothing, but all metrics are SQL functions.
- **Decision:** The metrics engine is a set of pure SQL files in `sql/engine/`, loaded into PGlite in the browser. Functions take transactions, prices and rates as arguments and never read user tables. Logged-in users also compute in the browser; Supabase is storage, RLS and market data only.
- **Consequences:**
  - One runtime: the PGlite SQL tests run exactly what production runs.
  - Engine is limited to PL/pgSQL, no extensions.
  - PGlite is lazy-loaded after upload so the WASM bundle does not hit the landing page.
  - Closing prices for the user's tickers must reach the browser (see ADR-007).
- **Rejected:** stateless RPC that computes on the server. Simpler and keeps prices server-side, but transactions leave the browser.

## ADR-002: Next.js 16 instead of 15

- **Status:** Accepted (2026-11-11).
- **Context:** The original brief named Next.js 15. The current stable line is 16 and the 15 support window has ended.
- **Decision:** Next.js 16 (App Router), React 19.

## ADR-003: No PostHog, Sentry or pg_cron until a result is on screen

- **Status:** Accepted (2026-11-11).
- **Context:** The metrics engine and the result page carry the project. Telemetry and scheduling do not.
- **Decision:** These tools enter only in the pilot phase. Market data is ingested by the CLI until then.

## ADR-004: Row hash includes an ordinal

- **Status:** Accepted (2026-11-11).
- **Context:** Deduplication by a hash of the normalized row drops real data: two identical trades on the same day produce identical rows.
- **Decision:** `row_hash = sha256(normalized row + ordinal among identical rows in the file)`. Unique on `(user_id, row_hash)`.
- **Consequences:** Re-importing the same file is idempotent. Overlapping exports that split a group of identical rows differently can still double count; the preview step shows the count of rows skipped as duplicates.

## ADR-005: Real statements are never committed

- **Status:** Accepted (2026-11-11).
- **Context:** Removing CPF and name does not anonymize a statement: tickers, dates and amounts identify a person.
- **Decision:** Real statements live in `fixtures/real/` (gitignored), used with the owner's consent. Only synthetic fixtures are committed and run in CI. Tests against real fixtures run locally and skip when the files are absent.

## ADR-006: No opening position in the MVP

- **Status:** Accepted (2026-11-11).
- **Context:** B3 history starts in Nov 2019. None of the three pilot investors held positions before that.
- **Decision:** The parser fails with a clear message if any position goes negative. Opening positions are a later feature.

## ADR-007: Price endpoint returns only requested tickers and dates

- **Status:** Accepted (2026-11-11), conditional on the B3 COTAHIST terms of use, to be read before Phase 3.
- **Context:** ADR-001 needs closing prices in the browser. B3 terms restrict redistribution of raw data.
- **Decision:** No direct `select` on `prices`. A rate-limited function returns closes only for the tickers and date range in the user's own statement. Never bulk.
- **Open risk:** whether this counts as redistribution. If it does, fall back to the stateless RPC rejected in ADR-001.

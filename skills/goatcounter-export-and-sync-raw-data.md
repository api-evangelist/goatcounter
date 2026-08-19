---
name: Export and incrementally sync raw pageview data
description: >-
  Run GoatCounter's asynchronous export, download the file once it finishes, and use the returned
  hit-ID cursor to sync only new data on every later run.
api: openapi/_original/goatcounter-api-swagger20.json
operations:
  - POST_api_v0_export
  - GET_api_v0_export_{id}
  - GET_api_v0_export_{id}_download
generated: '2026-08-13'
method: generated
source: https://www.goatcounter.com/help/api
---

# Export and incrementally sync raw pageview data

Exports are the only way to get raw hit-level data out of GoatCounter. They are **asynchronous** —
a submit / poll / download loop — and they carry a cursor that makes repeated syncs cheap.

Base host: `https://{code}.goatcounter.com/api/v0`. Headers on every call:
`Authorization: Bearer <token>` and `Content-Type: application/json`.

## Steps

1. **Start the export** with `POST_api_v0_export`. The body is a `handlers.apiExportRequest`:
   - `format` — `csv` or `json`.
   - `start_from_hit_id` — begin after this hit ID (the CSV cursor).
   - `start_from_day` — begin from this day (the JSON cursor).

   Omit both on a first full export. The response is **202** carrying a `v2.Export` object; keep
   its `id`.

   ```sh
   id=$(curl -s -X POST "$api/export" \
     -H 'Content-Type: application/json' -H "Authorization: Bearer $token" | jq .id)
   ```

2. **Poll** `GET_api_v0_export_{id}` until `finished_at` is non-null. The `v2.Export` object also
   carries `error`, `num_rows`, `size`, `hash` (SHA256) and — the field that matters for step 4 —
   `last_hit_id`.

   ```sh
   while :; do
     sleep 1
     finished=$(curl -s "$api/export/$id" -H "Authorization: Bearer $token" | jq .finished_at)
     [ "$finished" != "null" ] && break
   done
   ```

3. **Download** with `GET_api_v0_export_{id}_download`. This is the one operation that does not
   return JSON — it returns the export file (gzipped; the docs pipe it through `gzip -d`).

   Watch the status code here: this endpoint returns **202 with the error envelope** while the
   export is still running. A 202 is not a success. Only 200 is.

4. **Save the cursor.** Read `last_hit_id` from the finished export and pass it as
   `start_from_hit_id` on the next run. That turns every subsequent export into a delta rather
   than a full dump:

   ```sh
   start=$(curl -s "$api/export/$id" -H "Authorization: Bearer $token" | jq .last_hit_id)
   id=$(curl -s -X POST "$api/export" -H 'Content-Type: application/json' \
     -H "Authorization: Bearer $token" --data "{\"start_from_hit_id\":$start}" | jq .id)
   ```

## Rules that will bite you

- **Raw individual pageviews may not exist.** As of v2.6.0 GoatCounter no longer stores individual
  pageviews in the `hits` table by default — only aggregates. If the site has that setting off,
  there is nothing to export at hit level. It is re-enabled under *Settings → Data Collection →
  Individual pageviews*. When it is off, use the stats operations instead
  (`GET_api_v0_stats_total`, `GET_api_v0_stats_hits`).
- The poll loop counts against the **4 requests/second** rate limit. Sleep between polls; the worked
  example in the provider's docs sleeps one second.
- Errors are `{"error": "..."}` or `{"errors": {...}}`, never both, and only 400/401/403 are
  documented. The provider's own example notes it skips error checking for brevity — do not copy
  that part.
- Exports are per-site. A key scoped to one site cannot export another.

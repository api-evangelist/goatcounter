---
name: Build a custom dashboard from GoatCounter statistics
description: >-
  Read totals, per-path hits, referrals and category breakdowns from the GoatCounter stats
  endpoints, paginating each one correctly — they do not all paginate the same way.
api: openapi/_original/goatcounter-api-swagger20.json
operations:
  - GET_api_v0_stats_total
  - GET_api_v0_stats_hits
  - GET_api_v0_stats_hits_{path_id}
  - GET_api_v0_stats_{page}
  - GET_api_v0_stats_{page}_{id}
  - GET_api_v0_paths
generated: '2026-08-13'
method: generated
source: https://www.goatcounter.com/help/api
---

# Build a custom dashboard from GoatCounter statistics

Five read operations cover everything the GoatCounter dashboard itself renders. The provider's own
reference implementation is the `goatcounter dashboard` CLI command — the docs point at it instead
of a shell example because a script would be too convoluted.

Base host: `https://{code}.goatcounter.com/api/v0`. Headers:
`Authorization: Bearer <token>`, `Content-Type: application/json`.

## The five reads

| Want | Operation | Returns |
|---|---|---|
| Total visitors over a range | `GET_api_v0_stats_total` | `handlers.apiCountTotalResponse` — `total`, `total_events`, `total_utc`, `stats[]` per day/hour |
| Top pages | `GET_api_v0_stats_hits` | `handlers.apiHitsResponse` — `hits[]` of `goatcounter.HitList`, `total`, `more` |
| Where a page's traffic came from | `GET_api_v0_stats_hits_{path_id}` | `handlers.apiRefsResponse` — `refs[]` of `goatcounter.HitStat`, `more` |
| Browsers / systems / locations / languages / sizes / campaigns / referrers | `GET_api_v0_stats_{page}` | `handlers.apiStatsResponse` — `stats[]` of `goatcounter.HitStat`, `more` |
| Detail inside one of those buckets (e.g. a browser version) | `GET_api_v0_stats_{page}_{id}` | `handlers.apiStatsResponse` |
| The list of tracked paths, no statistics | `GET_api_v0_paths` | `handlers.apiPathsResponse` — `paths[]` of `goatcounter.Path`, `more` |

Common query parameters across the stats calls: `start`, `end` (date-time — **the docs say round
them to the hour**), `limit`, `include_paths`, `exclude_paths`, `path_by_name`, and `group`
(`day` | `week` | `month`).

## Pagination — three different shapes, one API

This is the thing that breaks naive clients. Every collection response carries a `more` boolean,
but how you ask for the next page differs by operation:

1. **`GET_api_v0_paths` uses an ID cursor.** Pass `after=<last id you saw>` with `limit`. Paths come
   back sorted by ID.
2. **`GET_api_v0_stats_hits` paginates by exclusion.** There is no offset. Pass the path IDs you
   have already received back in `exclude_paths` to get the next slice. Set `path_by_name=true` if
   you want to pass path names instead of IDs.
3. **`GET_api_v0_stats_hits_{path_id}`, `GET_api_v0_stats_{page}` and `GET_api_v0_stats_{page}_{id}`
   use a plain `offset`** with `limit`.

Always drive the loop off `more`, not off a page count.

## Reading the time series

`goatcounter.HitListStat` is the per-day bucket used by both `stats/total` and the `stats[]` inside
each `HitList`:

- `day` — the day these statistics are for
- `hourly` — array of visitors per hour
- `daily` — total visitors for that day
- `weekly` — set only once every 7 days
- `monthly` — set only on the first day of the month

`weekly` and `monthly` are sparse by design. Do not sum them; read them where they are set, or ask
for `group=week` / `group=month` instead.

## Rules that will bite you

- **`daily` as a query parameter is deprecated.** The spec says: "identical to `group=day` and will
  be removed in the future." Use `group=day`.
- `HitList.max` is the highest visitors per hour or day depending on grouping — useful for scaling
  a chart axis, meaningless as a total.
- `ref_scheme` on `HitList` and `HitStat` is only populated when you are retrieving referrals.
- The dashboard is chatty and the limit is **4 requests/second**. The provider notes their own
  dashboard fetches several of these in parallel — stay inside the limit and watch
  `X-Rate-Limit-Remaining`.
- Everything is scoped to the site the API key belongs to. To span several sites, list them with
  `GET_api_v0_sites` and hold a key per site.

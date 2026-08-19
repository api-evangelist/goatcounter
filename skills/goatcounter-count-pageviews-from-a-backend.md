---
name: Count pageviews from a backend
description: >-
  Send a batch of pageviews or events to GoatCounter from server-side code instead of the browser
  tracking script, without double-counting on retry.
api: openapi/_original/goatcounter-api-swagger20.json
operations:
  - POST_api_v0_count
generated: '2026-08-13'
method: generated
source: https://www.goatcounter.com/help/api
---

# Count pageviews from a backend

Use this when the pageview is known server-side — middleware, a logfile pipeline, an app backend —
and you do not want to rely on `count.js` in the browser. GoatCounter's docs recommend this
endpoint over the browser `/count` endpoint for backend use: it has higher rate limits, accepts
extra fields, and takes a batch in one request.

## Before you call

1. **Get the host right.** The API is served from the site's own subdomain:
   `https://{code}.goatcounter.com/api/v0`. `www.goatcounter.com/api/v0/*` returns
   `404 {"error":"not found"}` and `goatcounter.com/api/v0/*` returns a 301. A self-hosted instance
   uses its own hostname.
2. **Create an API key** in the dashboard under *[Username in top menu] → API*.
3. Every request needs both headers:
   - `Authorization: Bearer <token>`
   - `Content-Type: application/json`

## Steps

1. Build the batch. `POST_api_v0_count` takes a `handlers.APICountRequest` body:
   - `hits` — array of hit objects. Each hit accepts `path`, `title`, `ref`, `event`, `size`,
     `bot`, `user_agent`, `language`, `location`, `ip`, `created_at`, `query`, `session`.
   - `no_sessions` — set `true` when you cannot supply session information; GoatCounter then skips
     unique-visitor tracking for this batch.
   - `filter` — optional filtering directives.

   ```sh
   curl -X POST "https://MYCODE.goatcounter.com/api/v0/count" \
     -H 'Content-Type: application/json' \
     -H "Authorization: Bearer $token" \
     --data '{"no_sessions": true, "hits": [{"path": "/one"}, {"path": "/two"}]}'
   ```

2. Expect **202 Accepted with no body** on success. There is nothing to read back — GoatCounter
   exposes no read operation for an individual hit.

3. Handle the three documented failures. Every operation in this API declares exactly 400, 401 and
   403 and nothing else:
   - **400** — validation failed. The body is `{"error": "..."}` or
     `{"errors": {"field": ["..."]}}`. Read `errors` for the offending field names.
   - **401** — the key is missing or wrong.
   - **403** — the key is valid but lacks the required permission. Check
     `GET_api_v0_me`, whose `token.permissions` describes the key in use.

## Rules that will bite you

- **There is no idempotency key.** GoatCounter documents none and the spec declares none. If you
  retry a batch after a timeout you will double-count it. Deduplicate before you send: keep a
  client-side record of which batches were acknowledged, and only retry when you have a definite
  non-2xx or a connection failure with no response.
- **Rate limit is 4 requests/second**, reported on every response as `X-Rate-Limit-Limit`,
  `X-Rate-Limit-Remaining` and `X-Rate-Limit-Reset` (seconds until reset). The status code returned
  when you exhaust it is not documented, so treat any unexpected status as throttling, back off for
  `X-Rate-Limit-Reset` seconds, and batch harder rather than calling more often — batching is free,
  requests are not.
- A **500 requests/hour** limit on this endpoint class is listed as unreleased in the GoatCounter
  changelog. Design for it now.
- Errors never arrive as `application/problem+json`. Do not write an RFC 9457 parser for this API.

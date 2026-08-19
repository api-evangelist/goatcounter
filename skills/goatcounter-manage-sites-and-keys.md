---
name: Manage GoatCounter sites and verify API key permissions
description: >-
  List, create and update tracked sites, and check what the API key in hand is actually allowed to
  do before a call fails with 403.
api: openapi/_original/goatcounter-api-swagger20.json
operations:
  - GET_api_v0_me
  - GET_api_v0_sites
  - PUT_api_v0_sites
  - GET_api_v0_sites_{id}
  - PATCH_api_v0_sites_{id}
  - POST_api_v0_sites_{id}
generated: '2026-08-13'
method: generated
source: https://www.goatcounter.com/help/api
---

# Manage GoatCounter sites and verify API key permissions

A GoatCounter *site* is the unit of tenancy: it determines the API hostname, the API key, and every
piece of data you can read. Get this wrong and everything else 401s or 403s.

Base host: `https://{code}.goatcounter.com/api/v0`. Headers:
`Authorization: Bearer <token>`, `Content-Type: application/json`.

## Step 1 — always start with `GET_api_v0_me`

It is the cheapest possible call and it answers the two questions that cause most failures. The
response is a `handlers.meResponse`:

- `user` — a `goatcounter.User` (id, email, site, settings, totp_enabled, …)
- `token` — a `goatcounter.APIToken` with `name`, `permissions` and `sites`

If this returns **401**, the key is missing or wrong — check you are on the site's own subdomain and
not `www.goatcounter.com`. If a later call returns **403** but `/me` works, the key is valid and the
permission is the problem; regenerate the key in the dashboard under *[Username in top menu] → API*.

> The permission *values* are not enumerated in the public docs or the spec — `permissions` is typed
> as an integer with no published mapping. Treat it as opaque: read it for logging, but decide
> capability from whether a call returns 403, not by decoding the number.

## Step 2 — enumerate sites

`GET_api_v0_sites` returns `handlers.apiSitesResponse` with `sites[]` of `goatcounter.Site`. The
fields that matter:

- `code` — the domain code, e.g. `arp242`, which **is** the subdomain: `arp242.goatcounter.com`.
  This is how you compute the API host for that site.
- `cname` — custom domain, e.g. `stats.example.com`, and `cname_setup_at` once verified
- `link_domain` — the site being tracked, for linking out
- `parent` — set when this site is a child of another, which is how multi-domain accounts are modelled
- `received_data` — true after the first pageview arrives; the quickest integration health check
- `first_hit_at`, `created_at`, `updated_at`, `state`
- `setttings` — a `goatcounter.SiteSettings`

## Step 3 — create or update

- **Create:** `PUT_api_v0_sites` with a `goatcounter.Site` body. Note the verb — creation is PUT on
  the collection, not POST.
- **Read one:** `GET_api_v0_sites_{id}`.
- **Update:** `PATCH_api_v0_sites_{id}` or `POST_api_v0_sites_{id}`. Both are documented as "Update
  a site" on the same path and take a `handlers.apiSiteUpdateRequest` — `cname`, `link_domain`,
  `settings`. Prefer PATCH.

## Rules that will bite you

- **`setttings` is spelled with three t's** on the `goatcounter.Site` object in the published spec.
  That is the field name the API returns. Do not "fix" it in your client model.
- The update request object uses `settings` (two t's) while the site object returns `setttings`
  (three). They are not the same key. Map them explicitly.
- Site settings carry privacy-relevant switches — `allow_counter` (whether the public visitor-count
  widget works), `allow_embed`, `public`, `secret`, `data_retention`, `ignore_ips`,
  `collect`/`collect_regions`. Changing `public` or `allow_counter` changes what an unauthenticated
  visitor can see. Treat these as high-consequence writes and confirm with a human first.
- There is **no idempotency key**. A retried `PUT_api_v0_sites` can create a second site.
- Documented failures are 400, 401 and 403 only, in the `{"error": ...}` / `{"errors": {...}}`
  envelope — never problem+json.

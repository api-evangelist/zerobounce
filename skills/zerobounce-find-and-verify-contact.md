---
name: zerobounce-find-and-verify-contact
description: >-
  Find a person's work email address at a domain or company with ZeroBounce Email Finder,
  then verify it before using it. Covers the 20-credit price of a guess, the shared
  endpoint behind Email Finder and Domain Search, and the activity check that tells you
  whether anyone is actually reading.
api: zerobounce:email-validation-api
generated: '2026-08-13'
method: generated
source: >-
  collections/zerobounce-api-v2-official.postman_collection.json,
  mcp/zerobounce-mcp.yml, mcp/zerobounce-tool-crosswalk.yml,
  plans/zerobounce-plans-pricing.yml, errors/zerobounce-error-codes.yml
operations:
  - 'GET /v2/guessformat'
  - 'GET /v2/validate'
  - 'GET /v2/activity'
  - 'GET /v2/getcredits'
---

# Find and verify a contact address with ZeroBounce

## Cost first

An Email Finder query costs **20 credits** — twenty times a validation. This is the most
expensive single call in the API. Check the budget before looping over a list of names:

```
GET https://api-us.zerobounce.net/v2/getcredits?api_key=<key>
```

## 1. Find the address

```
GET https://api-us.zerobounce.net/v2/guessformat?api_key=<key>&domain=<domain>&first_name=<first>&middle_name=<middle>&last_name=<last>
```

`first_name` is the one name part you must supply, and you must supply **either** a
`domain` **or** a company to resolve one.

Email Finder and Domain Search are the **same endpoint**. ZeroBounce sells them as two
products and the MCP server exposes them as two tools (`find_email` and `domain_search`),
but both land on `/v2/guessformat`. The difference is only which parameters you send:

- **Domain Search** — `domain` (or company) alone. Returns the likely address *format*
  used at that domain.
- **Email Finder** — `domain` plus a person's name. Returns a specific candidate address.

If you only need the pattern for a domain, do not send a name — and do not call it twice.

## 2. Verify before you trust it

A found address is a **guess**. Validate it:

```
GET https://api-us.zerobounce.net/v2/validate?api_key=<key>&email=<found_address>
```

Then branch on `status` exactly as in `zerobounce-validate-single-email`. Pay particular
attention to two outcomes on found addresses:

- `catch-all` — very common on corporate domains, and it means the guess is **unproven**.
  The server accepts everything. Do not treat it as verified.
- `do_not_mail` / `sub_status: role_based` — Email Finder will happily surface
  `info@`, `sales@`, `contact@`. These deliver but damage reputation.

That verification costs 1 more credit, bringing the real cost of one usable contact to
**21 credits**.

## 3. Check whether anyone reads it

```
GET https://api-us.zerobounce.net/v2/activity?api_key=<key>&email=<address>
```

Activity Data tells you whether the address has been recently active. A `valid` address
with no activity is a deliverable dead end. Note that Activity Data is not included on the
Freemium tier — it requires ZeroBounce ONE.

Also watch for `sub_status: ai_agent_mailbox` on the validation result. It means the
mailbox is managed by an AI agent rather than monitored by a person, which changes what
"a human will read this" is worth.

## Doing this from an MCP client

The official ZeroBounce MCP server (`npm install -g @zerobounce/mcp`, local stdio only —
there is no hosted endpoint) exposes `find_email`, `domain_search`, `validate_email`,
`get_activity_data` and `get_credits`, which covers this whole flow. Its arguments are
camelCase (`firstName`, `lastName`) over the REST snake_case (`first_name`, `last_name`).
It additionally enforces that at least one of `domain` or `company` is present, a rule its
own JSON schema does not express.

Bulk Email Finder and Bulk Domain Search **file** operations exist in REST but have no MCP
tool — the MCP server wraps the Node SDK, and the SDK does not implement them. For bulk
finding you must call `https://bulkapi.zerobounce.net/email-finder/sendfile` directly.

## Failure handling

Standard surface: 400 malformed, 403 API Shield (not an auth failure), 429 rate limited
with no `Retry-After`, 500/520/524 platform. No idempotency key — a retried
`guessformat` call spends another 20 credits. Reconcile with `getcredits` rather than
blind-retrying.

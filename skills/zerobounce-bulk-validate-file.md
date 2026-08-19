---
name: zerobounce-bulk-validate-file
description: >-
  Run a bulk email-list validation job on ZeroBounce end to end — upload the CSV, track
  the file_id, receive or poll for completion, pull the results, and clean up. Covers the
  callback that has no signature and the retry that costs you the whole list twice.
api: zerobounce:email-validation-api
generated: '2026-08-13'
method: generated
source: >-
  collections/zerobounce-api-v2-official.postman_collection.json,
  https://www.zerobounce.net/docs/email-validation-api-quickstart/v2-send-file/,
  asyncapi/zerobounce-webhooks.yml, conventions/zerobounce-conventions.yml
operations:
  - 'POST https://bulkapi.zerobounce.net/v2/sendfile'
  - 'GET https://bulkapi.zerobounce.net/v2/filestatus'
  - 'GET https://bulkapi.zerobounce.net/v2/getfile'
  - 'GET https://bulkapi.zerobounce.net/v2/deletefile'
---

# Bulk-validate an email list with ZeroBounce

## When to use this

Above roughly a few hundred addresses, stop calling `/v2/validate` in a loop. The
real-time batch endpoint caps at 100 addresses per call and **30 requests per minute**;
the file pipeline has no such ceiling.

## Note the host change

Bulk operations do **not** run on the API host. They run on
`https://bulkapi.zerobounce.net`, which is not region-split and is not even listed on
ZeroBounce's own API-endpoints page. Do not assume the US/EU residency split applies here.

## 1. Upload the file

```
POST https://bulkapi.zerobounce.net/v2/sendfile
Content-Type: multipart/form-data
```

| Field | Required | Notes |
|---|---|---|
| `api_key` | yes | Account key |
| `file` | yes | CSV or TXT |
| `email_address_column` | yes | **1-based** column index |
| `return_url` | no | Completion callback URL |
| `first_name_column` | no | 1-based |
| `last_name_column` | no | 1-based |
| `gender_column` | no | 1-based |
| `ip_address_column` | no | Signup IP, improves geolocation |
| `has_header_row` | no | true/false |
| `remove_duplicate` | no | defaults to **true** |
| `allow_phase_2` | no | Enables Verify+ catch-all resolution |

Two traps here. Column indexes on the REST API start at **1**, but the MCP tool's
`emailAddressColumn` argument is documented as **0-based** — do not carry an index
between the two surfaces. And `remove_duplicate` defaults to true, so your output row
count will not match your input row count unless you set it false.

The response carries a `file_id`. **Persist it immediately.** There is no list-files
endpoint. If you lose the `file_id`, the job is unrecoverable through the API.

## 2. Wait for completion

Either register a `return_url` on upload, or poll.

**If you registered a `return_url`**, ZeroBounce POSTs JSON when the job finishes:

```json
{
  "file_id": "aaaaaaa-zzzz-xxxx-yyyy-5003727fffff",
  "file_name": "Your file name.csv",
  "upload_date": "2023-04-28T15:25:41Z"
}
```

That payload is **not signed**. There is no signature header, no shared secret and no
verification mechanism. Treat the callback purely as a hint to go pull the result with
your own API key — never as authenticated data, and never as the result itself. It
carries no validation outcomes at all.

**If you poll**, use:

```
GET https://bulkapi.zerobounce.net/v2/filestatus?api_key=<key>&file_id=<id>
```

## 3. Pull the results

```
GET https://bulkapi.zerobounce.net/v2/getfile?api_key=<key>&file_id=<id>
```

Returns CSV, not JSON. Each row carries the same `status` / `sub_status` vocabulary as the
single-validation response — see `zerobounce-validate-single-email` for how to branch on
it. Rows are not addressable resources; the file is the only handle.

## 4. Clean up

```
GET https://bulkapi.zerobounce.net/v2/deletefile?api_key=<key>&file_id=<id>
```

Note the verb: deletion is a **GET**, not a DELETE.

## The retry rule that matters

There is **no idempotency key anywhere in this pipeline**. Re-POSTing the same file
creates a second job and validates the whole list a second time, at 1 credit per address.
On a 200,000-row list that is a 200,000-credit mistake.

If a `sendfile` call times out or errors ambiguously, do **not** resend. Check
`GET /v2/getcredits` and, if you captured a `file_id`, check `filestatus`. Only resend
once you have positively established that no job was created.

ZeroBounce is explicit about the same hazard on callbacks: duplicate requests produce
duplicate callbacks, and each callback is charged.

## Error handling

Same HTTP surface as the rest of the API: 400 malformed, 403 API Shield (not an auth
failure — do not rotate the key), 429 rate limited with no `Retry-After`, 500/520/524
platform or transport. See `errors/zerobounce-error-codes.yml`.

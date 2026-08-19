---
name: zerobounce-validate-single-email
description: >-
  Validate one email address with ZeroBounce and decide correctly whether it is safe to
  send to. Covers the credit cost, the sandbox addresses that cost nothing, and the
  status/sub_status branching that HTTP 200 does not tell you about.
api: zerobounce:email-validation-api
generated: '2026-08-13'
method: generated
source: >-
  openapi/zerobounce-validation-api-openapi.yml,
  collections/zerobounce-api-v2-official.postman_collection.json,
  errors/zerobounce-error-codes.yml, sandbox/zerobounce-sandbox.yml,
  rate-limits/zerobounce-rate-limits.yml
operations:
  - validate
  - 'GET /v2/validate'
  - 'GET /v2/getcredits'
---

# Validate a single email address with ZeroBounce

## When to use this

You have one address and you need to know whether sending to it will bounce, hit a spam
trap, or damage sender reputation. Each call costs **1 credit**.

## Before you call

1. You need a ZeroBounce account API key. There are no scopes — the key has full account
   authority and can spend credits. Get it from the member dashboard.
2. Pick the right host. ZeroBounce splits by data residency and the choice is not
   cosmetic:
   - `https://api-us.zerobounce.net/v2` — US-only processing
   - `https://api-eu.zerobounce.net/v2` — EU-only processing
   - `https://api.zerobounce.net/v2` — **legacy**. ZeroBounce's own endpoints page says
     this host now serves EU traffic only. Do not use it for new US integrations.
3. Check your balance first if the caller is budget-sensitive:
   `GET /v2/getcredits?api_key=<key>`

## Call it

```
GET https://api-us.zerobounce.net/v2/validate?api_key=<key>&email=<address>&ip_address=<optional>
```

`ip_address` is optional and improves geolocation accuracy on the response.

The operation is also available on the ChatGPT-plugin host as operationId `validate`
(`POST https://members-api.zerobounce.net/api/validation/chatgpt-single-email-validation/`),
which accepts up to **3 unauthenticated requests per day** with no API key at all — useful
for a one-off check before any credential exists.

## Read the answer correctly

**This is the step agents get wrong.** A failed address returns HTTP **200**, not an
error. Never infer deliverability from the status code. Branch on `status`:

| `status` | What to do |
|---|---|
| `valid` | Safe to send. |
| `invalid` | Do not send. Remove from the list. |
| `catch-all` | Unproven. The domain accepts everything. Hold, or escalate to Verify+. |
| `spamtrap` | Do not send, ever. |
| `abuse` | Do not send. Known complainer. |
| `do_not_mail` | Deliverable but harmful — role, disposable, suppressed or toxic. |
| `unknown` | Retry later. The receiving server did not answer. |

Then read `sub_status` for the reason. Treat it as an **open** vocabulary: ZeroBounce
added `ai_agent_mailbox` on 2026-07-14 with no version bump, so a closed enum in your code
will break on the next addition. `ai_agent_mailbox` specifically means the mailbox is run
by an AI agent rather than watched by a person — relevant if the point of the message is
for a human to read it.

Retry only on `status: unknown`, and especially on `sub_status: greylisted`, which
ZeroBounce documents as often resolving on resubmission.

Every boolean-looking field (`free_email`, `mx_found`, `catchall_domain`) and
`domain_age_days` arrive as **strings**. Coerce them; do not compare against `true`.

## Test without spending credits

Send any of the reserved `@example.com` addresses with your **real** key. They cost zero
credits and return deterministic results:

`valid@example.com`, `invalid@example.com`, `catch_all@example.com`,
`spamtrap@example.com`, `abuse@example.com`, `donotmail@example.com`,
`unknown@example.com`, `greylisted@example.com`, `aiagentmailbox@example.com`.

Use `99.110.204.1` as `ip_address` to exercise the geolocation fields.

There is no separate test key and no sandbox host — you are calling production.

## Failure handling

- **429** — you exceeded 80,000 validations in 10 seconds (100,000 on ZeroBounce ONE).
  ZeroBounce then blocks you for **1 minute**. No `Retry-After` and no `RateLimit-*`
  headers are published, so back off on a fixed schedule.
- **403** — this is ZeroBounce's API Shield engaging, **not** a bad credential. Do not
  rotate the key. Wait and retry.
- **400** — malformed request. 100 of these in a minute gets you blocked for an hour.
- **404** — wrong path. 100 requests to non-existent endpoints in 2 minutes gets you
  blocked for an hour, so do not probe for endpoints.
- **500 / 520 / 524** — platform or transport error. Retry, then contact support.

There is **no idempotency key**. A retried call is a second call and spends a second
credit. Reconcile with `GET /v2/getcredits` rather than assuming.

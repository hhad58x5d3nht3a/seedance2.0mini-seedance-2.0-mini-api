# seedance-2.0-mini API (seedance2.0mini) — API guide with per-unit pricing

> **$0.01056 per second at 480P** — the cheapest video route on the platform, billed per generated second.

**[Model page](https://apimart.ai/model/seedance-2.0-mini)** · **[Live pricing](https://apimart.ai/pricing)** · **[Get an API key](https://apimart.ai/keys)**

Everything on this page refers to **seedance-2.0-mini** — also written **seedance2.0mini**, **seedance 2.0 mini** or **seedance-20-mini** — served through the OpenAI-compatible APIMart gateway at `https://api.apimart.ai/v1`.

## Pricing (observed, snapshot 2026-09-24)

| Tier | Price |
| --- | --- |
| `480P` | $0.0106 |
| `720P` | $0.0229 |

Billed per unit, pay as you go, **$1 minimum top-up**, no subscription and no free quota. Every task response returns `cost` / `credits_cost` so the charge can be checked per call.

## What that costs at scale

| Volume | Cost |
| --- | --- |
| 100 | $2.288 |
| 1000 | $22.88 |

Linear at the observed rate, no volume discount assumed.

## How to call it

```bash
export APIMART_API_KEY="<token>"
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  --data '{"model":"seedance-2.0-mini","prompt":"a modern cliffside villa at dusk, slow camera push","size":"16:9","n":1}'
```

Submit, keep the `task_id`, then poll `GET /v1/tasks/{id}` until `completed`. The response carries the image/video URL and the exact amount charged.

## Troubleshooting the first call

| Symptom | Cause | Fix |
| --- | --- | --- |
| `401` | key missing or truncated | re-copy from the console; header is `Authorization: Bearer $APIMART_API_KEY` |
| no balance | account has no credit | top up from $1 — there is no free tier on any model here |
| `429` | too many concurrent calls on one key | back off, retry with the same `Idempotency-Key` |
| `model` not found | wrong id or wrong tier field | copy the exact id `seedance-2.0-mini` (alias `seedance2.0mini`) from the table above |
| task `failed` | prompt filtered, or a reference URL expired | resubmit with a new `Idempotency-Key` |

## FAQ

**How is it billed?** Per generated second, by resolution.
**Which resolutions?** 480P, 720P.
**Is this a relay?** Yes — APIMart is a third-party gateway. Same OpenAI-compatible request shape, different billing and settlement from the vendor's direct API.

## Disclosure

This repository documents access through APIMart, a third-party API gateway, and is not affiliated with the model vendor. Prices are the dated snapshot above; the platform console is authoritative for billing.

---
name: Fetch and inspect Atla Insights traces
description: Retrieve AI-agent observability traces from the Atla Insights Data API — list with filters, look up by IDs, and fetch a single trace with full spans.
api: openapi/atla-insights-openapi.yaml
operations: [listTraces, getTracesByIds, getTraceById]
---

# Fetch and inspect Atla Insights traces

Use the Atla Insights Data API to read traces of instrumented AI-agent runs.

## Auth
- Bearer token in the `Authorization` header (`Authorization: Bearer <API_TOKEN>`).
- Base URL: `https://app.atla-ai.com`.

## Steps
1. **List traces** — `listTraces` (`GET /api/sdk/v1/traces`). Narrow with
   `startTimestamp`/`endTimestamp` (ISO date-time), `metadataFilter`
   (URL-encoded JSON array of `{"key","value"}` pairs), and paginate with
   `page` (default 1) and `pageSize` (default 50, max 1000). Response returns a
   `traces` array.
2. **Resolve specific traces** — `getTracesByIds` (`GET /api/sdk/v1/traces/ids`)
   to fetch complete data for a known set of trace IDs in one call.
3. **Inspect one trace** — `getTraceById` (`GET /api/sdk/v1/traces/{traceId}`)
   for full trace detail including all `spans`, annotations, summary, and
   custom metric values.

## Conventions & errors
- Page-number pagination (see `conventions/atla-conventions.yml`).
- Handle `401` (invalid/missing token), `404` (unknown traceId, single-trace
  endpoint only), and `500` (retry) — see `errors/atla-problem-types.yml`.
- Read-only API: no idempotency key required.

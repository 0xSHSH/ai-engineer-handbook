# 1️⃣1️⃣ API Design Guide

> Part of the [Interview Handbook](README.md).

## 📑 Contents
- [REST](#rest)
- [GraphQL](#graphql)
- [WebSockets](#websockets)
- [Authentication: JWT & OAuth](#authentication-jwt--oauth)
- [Rate Limiting](#rate-limiting)
- [Pagination](#pagination)
- [Caching](#caching)
- [Versioning](#versioning)
- [Interview Questions](#interview-questions)

---

## REST
Principles: resources are nouns (`/orders/123`, not `/getOrder`), HTTP verbs carry the action (`GET`/`POST`/`PUT`/`PATCH`/`DELETE`), and responses use standard status codes.

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request (client error, malformed input) |
| 401 | Unauthorized (missing/invalid auth) |
| 403 | Forbidden (authenticated but not permitted) |
| 404 | Not Found |
| 409 | Conflict (e.g., duplicate resource, version mismatch) |
| 429 | Too Many Requests (rate limited) |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

**PUT vs PATCH**: PUT replaces the full resource (idempotent), PATCH applies a partial update (may or may not be idempotent depending on the patch semantics).

## GraphQL
Single endpoint, client specifies exactly the fields it needs in the query — solves REST's over-fetching (getting unused fields) and under-fetching (needing multiple round trips) problems.
```graphql
query {
  user(id: "123") {
    name
    orders(limit: 5) {
      id
      total
    }
  }
}
```
**Trade-offs vs REST**: more flexible for complex/nested client needs, but harder to cache at the HTTP layer (single endpoint, POST-based queries), and needs care to avoid the "N+1 resolver" problem (solved with batching tools like DataLoader).

## WebSockets
Full-duplex, persistent connection — needed when the server must push data without the client polling (chat, live notifications, streaming LLM tokens, collaborative editing). Compare to **SSE (Server-Sent Events)**: SSE is simpler and sufficient for server-to-client-only streaming (e.g., LLM token streaming) and works over plain HTTP; use WebSockets when the client also needs to push frequent messages back.

## Authentication: JWT & OAuth
- **JWT (JSON Web Token)**: a signed (and optionally encrypted) token containing claims (user id, expiry, scopes). Server verifies the signature without a DB lookup — stateless, fast, but revocation is hard (a compromised token is valid until it expires unless you maintain a blocklist).
- **OAuth 2.0**: authorization framework — lets a user grant a third-party app limited access without sharing their password. Key flows: **Authorization Code** (standard for web apps, involves a redirect and a backend token exchange), **Client Credentials** (service-to-service, no user involved).
- **Session-based auth**: server stores session state (in DB/Redis), client holds an opaque session ID cookie — easy to revoke, but requires server-side state (less horizontally scalable without a shared session store).

## Rate Limiting
| Algorithm | Idea | Trait |
|---|---|---|
| Fixed window | Count requests per fixed time bucket | Simple but allows bursts at window boundaries (2x limit possible right at the edge) |
| Sliding window | Weighted count across overlapping windows | Smoother, more accurate, slightly more compute |
| Token bucket | Bucket refills at a fixed rate, each request consumes a token | Allows controlled bursts up to bucket size |
| Leaky bucket | Requests processed at a fixed output rate regardless of input burst | Smooths traffic to a constant rate |

Return `429 Too Many Requests` with a `Retry-After` header so well-behaved clients back off correctly.

## Pagination
- **Offset-based**: `?limit=20&offset=40` — simple, but slow on large offsets (DB still scans skipped rows) and unstable if rows are inserted/deleted between pages.
- **Cursor-based**: `?limit=20&cursor=<opaque_id>` — stable under concurrent writes, O(1)-ish regardless of depth, standard for infinite-scroll feeds and large datasets.

## Caching
- **Client-side**: `Cache-Control`, `ETag` (conditional requests — server returns 304 Not Modified if unchanged, saving bandwidth).
- **Server-side**: Redis/Memcached in front of the DB for hot reads.
- **CDN**: edge caching for static or rarely-changing public responses.
- Cache invalidation strategies: TTL expiry (simple, some staleness), explicit invalidation on write (fresher, more complex to get right across services).

## Versioning
| Strategy | Example | Trade-off |
|---|---|---|
| URI versioning | `/v1/orders` | Explicit, easy to route, clutters the URL |
| Header versioning | `Accept: application/vnd.api.v2+json` | Clean URLs, less discoverable |
| Query param | `/orders?version=2` | Simple, easy to default |

Always support the previous version for a deprecation window and communicate sunset dates — breaking a live API without notice is the fastest way to lose integration partners' trust.

---

## Interview Questions
1. When would you choose GraphQL over REST, and what do you give up?
2. Design a rate limiter for a public API with per-user and per-IP limits.
3. How would you handle API versioning for a service with thousands of external integrators?
4. Explain the difference between authentication and authorization, and where JWT fits.
5. Why is cursor-based pagination preferred over offset-based for large, frequently-changing datasets?
6. How would you design idempotency for a payment API so a client's network retry doesn't double-charge a user? (Idempotency keys: client sends a unique key per logical operation; server stores the key with the result and returns the cached result on retry instead of reprocessing.)

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [Cloud Guide](12_cloud_guide.md).*

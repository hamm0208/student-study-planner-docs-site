---
sidebar_position: 3
---
# Local Caching
This document describes the client-side caching implementation used in the project, how it works, how to use it, and important caveats.

## Overview

- Caching is implemented via `src/utils/db/DataCacher.js` and used by the frontend auth helper `src/utils/auth/FrontendAuthHelper.js` (exported class `SecureFrontendAuthHelper`).
- Storage backend: localforage instance named `StudentStudyPlannerCache` (works across browsers with IndexedDB/WebSQL/localStorage fallbacks).
- Purpose: reduce repeated GET API calls, speed up UI, and automatically invalidate related caches on mutating requests.

## Key concepts

- Cache key: derived from request URL via `CleanURL(url)`. It normalizes host, ensures leading `/`, and sorts query params for deterministic keys.
- Base resource: for invalidation the code derives a base API path like `/api/students` from a request. This is used to find & remove related cached entries (list endpoints, filtered lists, sub-resources).
- TTL: default TTL configured in `DataCacher.DEFAULT_TTL_SECONDS` (value: `180` seconds). Note: there is an incorrect comment saying "Cache for 1 day" — the code sets 180s (3 minutes). Update TTL if a different policy is desired.
- Returns:
  - `GetCache(url)` returns cached data or `null` on miss/error/expired.
  - `SetCache(url, data, ttl?)` stores data and expiry timestamp.
  - `RemoveInvalidationKey(url)` finds matching keys and removes them; returns list of invalidated keys.
  - `ClearCache(key)` and `ClearAllCache()` remove entries.

## Flow & integration

- Read-path (GET requests):
  - `SecureFrontendAuthHelper.authenticatedFetch(url, options)` checks if request is GET (or missing method).
  - If GET: it calls `DataCacher.GetCache(url)` first. If cached, it returns a mocked Response object (so callers can call `.json()`).
  - If no cached entry: it performs the fetch; if response OK it clones and caches the JSON result via `DataCacher.SetCache(url, data)`.

- Write-path (non-GET requests):
  - On non-GET, `authenticatedFetch` calls `DataCacher.RemoveInvalidationKey(url)` to remove cached entries related to the resource being modified.
  - This ensures subsequent GETs for that resource will retrieve fresh data.

- Authentication & errors:
  - In non-dev mode, requests include `x-session-email` and `Authorization: Bearer <sessionToken>`.
  - On 401/403 the auth helper:
    - Shows a Swal warning, clears all cache, and calls logout flow (via MSAL).
    - Cache cleared via `DataCacher.ClearAllCache()`.

## Functions (quick reference)

- DataCacher
  - async GetCache(url) -> data | null
  - async SetCache(url, data, ttlSeconds = DataCacher.DEFAULT_TTL_SECONDS) -> boolean
  - async RemoveInvalidationKey(url) -> array of invalidated keys
  - async ClearCache(key) -> boolean
  - async ClearAllCache() -> boolean

- SecureFrontendAuthHelper (relevant)
  - authenticatedFetch(url, options) -> Response (may be mocked for cache hits)
  - refreshSession(), logout(), getCurrentUser(), helper role checks

## Cache key behavior

- Keys are normalized to a leading slash and query params are sorted and URL-encoded.
- Example canonicalization:
  - Input: `https://api.example.com/api/students?b=2&a=1`
  - Clean key: `/api/students?a=1&b=2`
- For local resource invalidation `RemoveInvalidationKey` uses `_getBaseResourcePath` to compute `/api/<resource>` and invalidates any stored keys that start with that base (covers list, filtered list, and sub-resources like `/api/students/123/history`).

## Usage examples

- Typical component using `authenticatedFetch` (caching is automatic for GET):

```javascript
// example usage in a React effect
const loadStudents = async () => {
  const resp = await authenticatedFetch('/api/students?status=active');
  if (resp.ok) {
    const students = await resp.json();
    setStudents(students);
  }
};
```

- Manually interacting with DataCacher:

```javascript
import DataCacher from '@utils/db/DataCacher';
const cacher = new DataCacher();

await cacher.SetCache('/api/notes?user=1', { items: [] }, 60); // 60s TTL
const cached = await cacher.GetCache('/api/notes?user=1'); // returns data or null
await cacher.RemoveInvalidationKey('/api/notes/123'); // invalidates base /api/notes
```

## Recommendations & caveats

- TTL mismatch: update `DataCacher.DEFAULT_TTL_SECONDS` and code comment to reflect intended TTL (e.g., 86400 for 1 day).
- Be careful with sensitive data: the caching layer stores JSON responses in browser storage. Do not cache sensitive PII, tokens, or server responses that must remain confidential.
- Cache size & browser quota: localforage stores data in browser-managed stores; large payloads can hit quota limits. Consider reducing TTL or limiting what is cached.
- Server must be authoritative: client-side role checks and caching are for UI/performance only — server must enforce authorization.
- Dev mode: frontend has a dev override (NEXT_PUBLIC_MODE === 'DEV'). Dev mode may bypass auth and supply mock users; caching still occurs unless you explicitly change behavior.
- Storage event: components using `useSecureAuth` listen for `storage` changes to detect logout in other tabs.

## Troubleshooting

- No cache hits:
  - Ensure the URL you request matches normalized key (order of query params may differ; CleanURL sorts params).
  - Check console logs from `DataCacher` (it logs cache set/expiry/invalidation).
- Unexpected stale data:
  - Confirm TTL, look for successful `RemoveInvalidationKey` on write flows.
  - Check that mutating endpoints are non-GET so invalidation triggers.
- Quota/storage errors:
  - Get errors in console from localforage; treat as cache miss (no app-breaking behavior).

## Notes

- The mock Response returned for cache hits implements .json(), .text(), .clone(), `.ok` and `.status`, so callers expecting a standard fetch Response should work without changes.
- Consider adding optional per-endpoint TTLs for large or frequently-changing resources.
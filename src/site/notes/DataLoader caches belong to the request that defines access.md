---
{"dg-publish":true,"permalink":"/data-loader-caches-belong-to-the-request-that-defines-access/","title":"DataLoader caches belong to the request that defines access","hideInFiletree":true,"tags":["database","programming"],"noteIcon":"","dg-note-properties":{"title":"DataLoader caches belong to the request that defines access","categories":["Databases"],"tags":["database","programming"],"created":"2026-09-09","updated":"2026-09-09"}}
---


A cached record is not automatically valid for every caller who asks for the same identifier. When access differs by user, sharing a loader cache across requests can return data under the wrong access context.

The [DataLoader documentation](https://raw.githubusercontent.com/graphql/dataloader/main/README.md) recommends creating loader instances per request and warns against reusing one instance across requests from different users. Its cache is request-local memoization, not a replacement for a shared application cache. Fetching through the loader must still respect the caller's permissions.

Batching and caching solve different parts of [[References/N plus one problem\|N plus one problem]]. Batching groups different keys into fewer backend operations; caching avoids loading an already-requested key again. Neither makes a cached value universally shareable.

This supports [[Authentication does not replace authorization on each request\|Authentication does not replace authorization on each request]]: reducing repeated loads must not turn a result fetched for one caller into authority for another. Keep the loader lifetime aligned with the request context, and treat any broader cache as a separate design with explicit access and freshness rules.

# HTTP Cache
[[doc]](https://web.dev/http-cache/)

It's designed to avoid unnecessary network requests. And the behavior is controlled a combination of request/response headers.

## Terminology
- Revalidate: Asking server if there's a newer version of resource
- Fresh & Stale: Resource status
<pre>
       0            120s
       |---(fresh)---|---(stale)--->
    request      max-age=120
     fired
</pre>

## Request Headers
Most of time they're automatically appended to requests by the browser; used to checking for freshness (revalidate).
- `If-Match` (with `ETag` value)
    > The common case is a `GET` or `HEAD` request with [`Range`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Range) and `If-Match` headers to make sure getting the different part of the same resource.
- `If-None-Match` (with `ETag` value)
- `If-Modified-Since` (with `Last-Modified` value)
- ...

But if you need more control over default behavior, use [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) instead.

## Response Headers
- `Cache-Control`: To specify how, and for how long, the browser and other intermediate caches should cache the response.
- `ETag`: A token (usually a hash of contents) used to validate if the resource is different.
- `Last-Modified`: Serving the same purpose as `ETag` (content-based) but time-based.
- ...

Leaving out the `Cache-Control` response header does not disable HTTP caching. The browser has a [default behavior](https://www.mnot.net/blog/2017/03/16/browser-caching#heuristic-freshness).

### Cache-Control
#### Directives
- `no-store`: This instructs the browser and other intermediate caches (like CDNs) to never store any version of the file.
- `no-cache`: This instructs the browser that it *must revalidate with the server every time* before using a cached version of the URL. (ex: index.html)
- `must-revalidate`: Indicates that once a resource becomes stale, caches must not use their stale copy without successful validation on the origin server.
    > Used to prevent using stale resources in some cases like network is unavailable or server has no response

    > Difference between `no-cache` and `must-revalidate`:
    > - `no-cache` revalidate every time before using
    > - `must-revalidate` only revalidate after resource is stale
- `private`: Browsers can cache the file but intermediate caches cannot. (ex: private user-relative data)
- `public`: The response can be stored by any cache.
- `max-age=<seconds>`: The maximum amount of time a resource is considered fresh. Unlike [`Expires`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires) header, this directive is relative to the time of the request.
- ...

#### Cache-Control Strategy
![Cache-Control Strategy](../images/cache-control.png)

#### Examples
1. Get a resource and cached for 2 minutes since the request fired.
![max-age](../images/max-age.png)

2. Get the same resource again within 2 minutes. (Responds with `304 Not Modified`)
![If-None-Match](../images/if-none-match-etag.png)

## Cache Invalidation
**Versioned URLs** are a good practice because they make it easier to invalidate cached responses.

> - *.../resource?version=1.2.3*
> - *.../styles.x23d3dd.css*  
    (Can be automatically done by build tool like *webpack*)

Changing the URL of the resource will force the browser to download the new response whenever its content changes.

### Which To Use
- Long-lived caching for versioned URLs, ex: `Cache-Control: max-age=31536000` (one year).
- Server revalidation for unversioned URLs
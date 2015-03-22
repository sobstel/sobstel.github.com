---
title: "Debugging Akamai"
---

To debug Akamai-cached content you simply need to send properly formed `Pragma` header.

### Production example

    curl -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" -IXGET http://www.fifa.com/

### Staging example

    curl -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" -H "Host: www.fifa.com" -IXGET http://wildcard.fifa.com.edgesuite-staging.net/

Note `Host` header.

### Response headers

#### X-Check-Cacheable

Example: `X-Check-Cacheable: YES`

Informs whether resource is cacheable by Akamai.

#### X-Cache

Example: `TCP_MEM_HIT from a80-239-171-156 (AkamaiGHost/6.9.4-9930199)`

`a80-239-171-156` is Akamai server that returned output (80.239.171.156).

* TCP_HIT: the object was fresh in cache and object from disk cache.
* TCP_MISS: the object was not in cache, server fetched object from origin.
* TCP_REFRESH_HIT: The object was stale in cache and we successfully refreshed with the origin on an `If-Modified-Since` request.
* TCP_REFRESH_MISS: Object was stale in cache and refresh obtained a new object from origin in response to our `IF-Modified-Since` request.
* TCP_REFRESH_FAIL_HIT: Object was stale in cache and we failed on refresh (couldn't reach origin) so we served the stale object.
* TCP_IMS_HIT: `IF-Modified-Since` request from client and object was fresh in cache and served.
* TCP_NEGATIVE_HIT: Object previously returned a "not found" (or any other negatively cacheable response) and that cached response was a hit for this new request.
* TCP_MEM_HIT: Object was on disk and in the memory cache. Server served it without hitting the disk.
* TCP_DENIED: Denied access to the client for whatever reason.
* TCP_COOKIE_DENY: Denied access on cookie authentication (if centralized or decentralized authorization feature is being used in configuration).

#### X-Cache-Key

Example: `X-Cache-Key: /L/1758/81636/2m/www.fifa.com/`

`81636` is a CP code, `2m` is cache TTL at Akamai side (it may be different when set at application level through `Edge-Control` header).




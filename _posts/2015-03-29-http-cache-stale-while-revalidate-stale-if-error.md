---
title: "HTTP Cache: stale-while-revalidate and stale-if-error"
---

There are possible 2 [HTTP Cache-Control extensions for stale content][1rfc5861]:
stale-while-revalidate and stale-if-error. They are actually already (or being)
adopted by [Fastly][fastly] and [Chrome](chrome).

### stale-while-revalidate

When cache expires, user can be server stale content while the latest version is being
re-fetched in the background. It makes user never wait for content re-generation, so he
usually gets content immediately from cache.

    Cache-Control: max-age=3600, stale-while-revalidate=7200

Content is cached for 1 hour, but up to 2 hours it's allowed to serve stale content
if the latest version is not fetched yet.

Exact behaviour is explained in [proposed standard](http://tools.ietf.org/html/rfc5861#section-3).

### stale-if-error

When cache expires, user can be served stale content when web site is down. Even if
you're not fond of serving stale content, it's always better than broken page, isn't it?
It makes your website more fault tolerant.

    Cache-Control: max-age=3600, stale-while-revalidate=7200, stale-if-error=86400

Content is cached for 1 hour, but up to 24 hours it's allowed to serve stale content
if web site is down or page is broken.

Exact behaviour is explained in [proposed standard](http://tools.ietf.org/html/rfc5861#section-4).


[rfc5861]: http://tools.ietf.org/html/rfc5861
[fastly]: http://www.fastly.com/blog/stale-while-revalidate/
[chrome]: https://code.google.com/p/chromium/issues/detail?id=348877

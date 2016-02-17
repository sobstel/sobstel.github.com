---
title: "Optimizing for mobile (in a nutshell)"
---

Problem
-------

* Bandwidth
* Latency

Solution
--------

* Reduce page weight
* Reduce the number of requests
* Optimize when content is loaded (so website is usable more quickly)

Namely...

* Enable mod_gzip or mod_deflate to compress files
* Make sure assets are cached by the browser
* Minify assets
* Combine multiple assets together (concatenate JS/CSS files, use sprites for images)
* Inline small JS and CSS files
* Optimize images
* Defer loading when possible, eg. JS defer/async or put right before closing </body>
* Embed small images and fonts using the Data URI scheme
* User localStorage as a browser cache (when it makes sense)
* Drop big frameworks (only when you know what you're doing)

That's not all, but that's a good start ;-)

Tools
-----

* [YSlow](http://yslow.org/)
* [Mobitest](http://mobitest.akamai.com/)
* [Web Page Performance test](http://www.webpagetest.org/)

References
----------

* [The Mobile Book](https://shop.smashingmagazine.com/products/the-mobile-book-deluxe-bundle)

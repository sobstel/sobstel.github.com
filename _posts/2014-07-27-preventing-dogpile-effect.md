---
title: "Preventing the Dogpile Effect"
---

### Dogpile effect - theory (problem and solution)

Implementing caching in web apps seems to be simple. You check if value is cached. If it is, you fetch cached value from cache and serve it. If it's not, you generate new value and store in cache for future requests. Simple like that.

However, what if value expires and then you get hundreds of requests? It cannot be served from cache anymore, so your databases are hit with numerous processes trying to re-generate the value. And the more requests databases receive, the slower and less responsive they get. Load spikes. Until eventually they likely go down.

See picture below (green - in cache, red - no cache).

<img src="/img/mgil-dogpile-effect-2010.jpg" width="600" height="262"/>

Preventing the dogpile effect boils down to having just one process (first one to come) regenerating new value while other subsequent processes serving stale value from cache until it's refereshed by the first process.

Worried about serving stale data? Well, if your databases are overloaded and suffering, serving stale data is the smallest inconvenience you can have. And if takes long to regenerate new value, having multiple processes doing this (instead of one) won't help really. It will just add more load.

### Dogpile effect - prevention/implementation

Dogpile effect can be prevented using semaphore lock. If value expires, first process acquires a lock and starts generating new value. All the subsequent requests check if lock is acquired and serve stale content. After new value is generated, lock is released.

Important to note is that in fact values should be given an extended life time, so they're not physically removed when they expire and they can be still served if there's a need.

Here's how it works in detail.

_Get cache value from cache store._

<pre><code>$value = $this->store->get($key);
</code></pre>

<code>$value</code> is a [value object](https://github.com/sobstel/metaphore/blob/master/src/Value.php).

_Check whether cached value expired or not. If not expired, serve it._

<pre><code>if ($value && !$value->isStale()) {
	return $value->getResult();
}
</code></pre>

_Otherwise, acquire lock so there's just one process regenerating new value._

<pre><code>$lock_acquired = $this->acquireLock($key, $grace_ttl);
</code></pre>

_If lock cannot be acquired, it means there's already other process regenerating it, so let's just serve current (stale) value._

<pre><code>if (!$lock_acquired) {
	return $value->getResult();
}
</code></pre>

_Otherwise (lock has been acquired), regenerate new value._

<pre><code>$result = ...
</code></pre>

_Save regenerated value in cache store. Add grace period, so stale result might be served if needed by other processes._

<pre><code>$expiration_timestamp = time() + $ttl;
$value = new Value($result, $expiration_timestamp);

$real_ttl = $ttl + $grace_ttl;
$this->store->set($key, $value, $real_ttl);
</code></pre>

_Release lock._

<pre><code>$this->releaseLock($key);
</code></pre>

Full implementation: [https://github.com/sobstel/metaphore/blob/master/src/Cache.php](https://github.com/sobstel/metaphore/blob/master/src/Cache.php).

### Metaphore

Metaphore is open-sourced library to prevent dogpile effect in PHP apps. It's actually rewrite of [LSDCache](https://github.com/gsmlabs/LSDCache), which has been successfully used in many high-traffic production web apps. I just believe that LSDCache has grown too big into multi-purpose cache library while metaphore strives to be simple to do just one thing and to do it well.

Usage is really simple.

In composer.json file:

<pre><code>"require": {
	"sobstel/metaphore": "dev-master"
}
</code></pre>

In your PHP file:

<pre><code>use Metaphore\Cache;

// initialize $memcached object (new Memcached())

$cache = new Cache($memcached);
$cache->cache($key, function(){
    // generate content
}, $ttl);
</code></pre>

### More reading

* [High Scalability - Strategy: Break Up The Memcache Dog Pile](http://highscalability.com/blog/2009/8/7/strategy-break-up-the-memcache-dog-pile.html)
* [Memcached - Avoiding stampeding herd](https://code.google.com/p/memcached/wiki/NewProgrammingTricks#Avoiding_stampeding_herd)
* [LeaseWeb Labs - Avoiding the Memcache ‘dog pile’ effect](http://www.leaseweblabs.com/2013/03/avoiding-the-memcache-dog-pile-effect/)
* [mysqlnd-qc slam defense](http://php.net/manual/en/mysqlnd-qc.slam-defense.php)
* [Dog-pile Effect and How to Avoid it with Ruby on Rails memcache-client Patch](http://kovyrin.net/2008/03/10/dog-pile-effect-and-how-to-avoid-it-with-ruby-on-rails-memcache-client-patch/)
* [How to avoid the dog-pile effect on your Rails app](http://blog.plataformatec.com.br/2009/09/how-to-avoid-dog-pile-effect-rails-app/)
* [MintCache](https://djangosnippets.org/snippets/155/)
* [Varnish Grace example](https://www.varnish-cache.org/trac/wiki/VCLExampleGrace)

### Thanks

Thanks to [Mariusz Gil](http://mariuszgil.com/) for his talk about Memcached back in 2010 at PHPCon - which made me aware of dogpile effect issue - and for allowing me to use pics from slides.

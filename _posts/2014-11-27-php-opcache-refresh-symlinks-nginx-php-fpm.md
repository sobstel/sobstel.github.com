---
title: "PHP opcache refresh when deploying using symlinks in nginx/php-fpm environment"
---

So...

* You're deploying using symlinks? (eg. capistrano, capifony)
* You serve your website using nginx and php-fpm?
* You have PHP [OPcache](http://php.net/manual/en/book.opcache.php) enabled?

...and you experience WTF case when your content somehow seems not to be refreshed despite calling `opcache_reset()`.

Reasons and solutions are described in the [ZendOptimizerPlus issue #126](https://github.com/zendtech/ZendOptimizerPlus/issues/126).

It can be solved by using `$realpath_root` in the nginx config:

```
fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
fastcgi_param DOCUMENT_ROOT $realpath_root;
```

It worked for me and huge thanks go to [Vitaly Chirkov](http://stackoverflow.com/questions/23737627/php-opcache-reset-symlink-style-deployment/23904770#23904770).

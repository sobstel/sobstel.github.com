---
title: "PHP gettext caching workaround"
---

<a href="http://www.gnu.org/software/gettext/">Gettext</a> is probably the most flexible, the most convenient, and the fastest way to handle <a href="http://en.wikipedia.org/wiki/I18N">I18N</a> in PHP applications (and not only PHP of course).

However, when you have PHP installed as an Apache module (mod_php), <a href="http://php.net/gettext">PHP's gettext extension</a> translation files are cached, so updates are not instantly visible. Unfortunately, there are no configuration directives to control caching time and practically the only way to clear the cache is to restart the web server. Obviously it can be a serious issue, especially on shared hosts, where you rarely have control over the server.

Fortunately, there's a little workaround, which makes the webserver restart needless.

<strong>Simply change your domain and translation files names every time you make the update.</strong>

Typical update procedure would be :

- Change names of .mo (and .po) files, e.g. from <code>domain1</code> to <code>domain2</code>.
- Update domain names in your code to <code>domain2</code>.

<pre>
# configuration file
$currentDomain = 'domain2'; #changeable

# somewhere in your code
$domain = $config->getCurentDomain();

bindtextdomain($domain, '/path/to/locale');
textdomain($domain);
</pre>

- Upload/commit changes.

You can consider writing some shell script to automate this as it can be pretty tedious task to do it manually over and over again.


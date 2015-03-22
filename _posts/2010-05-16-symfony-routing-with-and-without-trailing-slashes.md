---
title: "Symfony routing with and without trailing slashes"
---

<strong>UPDATE:</strong> Symfony2 version: <a href="http://symfony.com/doc/current/cookbook/routing/redirect_trailing_slash.html">The Cookbook Routing Redirect URLs with a Trailing Slash</a>.

Symfony routing, by default, seem not work with URLs with trailing slashes. 

For example <a href="http://www.symfony-project.org/community">www.symfony-project.org/community</a> works ok, but <a href="http://www.symfony-project.org/community/">www.symfony-project.org/community/</a> results in error 404.

You can fix it easily at application level by overriding <code>sfPatternRouting</code>.

<pre class="brush:php">
class myPatternRouting extends sfPatternRouting 
{
  public function parse($url) 
  {
    $url = rtrim($url, '/'); # trim trailing slashes before actual routing
    return parent::parse($url);
  }  
}
</pre>

Put this code in <code>lib</code> directory in <code>myPatternRouting.class.php</code> file and don't forget to adjust your settings in <code>factories.yml</code>.

<pre>
all:
  routing:
    class: myPatternRouting
    param:
      generate_shortest_url: true
      extra_parameters_as_query_string: true
</pre>

That's it.

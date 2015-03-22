---
title: "Doctrine2: re-usable logic in entities"
---

Old Doctrine1-style behaviours are gone. Entities are just plain old PHP objects. Great. There a <a href="http://www.doctrine-project.org/blog/doctrine2-behaviours-nutshell">few</a> <a href="http://www.doctrine-project.org/blog/doctrine2-versionable">blog</a> <a href="http://www.doctrine-project.org/blog/doctrine2-behavioral-extensions">posts</a> about how to implement so-called behaviours in Doctrine2.

But what if you'd like to just share same logic (methods) between models. 

Inheritance is not an answer here. First of all, <a href="http://en.wikipedia.org/wiki/Composition_over_inheritance">composition over inheritance</a>. Secondly, plain old (and dumb) simple PHP inheritance just don't work in Doctrine2. When I had 3-level inheritance, it resulted in some malformed queries being created. 

Mixins would do the work, but there's nothing like this in PHP. We'll have traits in PHP 5.4, so if you can wait for stable release or you're crazy enough to use alpha release, then just stop reading now.

For now, probably the best way will be to <b>auto-generate entities</b> (I would be delighted if someone can provoe me wrong) and guys behind <em>Doctrator</em> have came to same conclusion. This lib provides flexible way to generate similar classes and means to write powerful re-usable extensions. I do recommend to see <a href="<a href="http://www.slideshare.net/pablodip/doctrator-symfony-live-2011-paris">Doctrator Symfony Live 2011 Paris slides</a> for better view. 

Doctrator is pretty ok, I just don't like the way you need to define custom methods, eg.

<pre>
$setterName = 'set'.Inflector::camelize($name);
$setter = new Method('public', $setterName, '$value', '$this->$name = $value;');
$this->definitions['entity']->addMethod($setter);
);
</pre>

Turning method body into string to pass it as constructor argument isn't really too convenient, is it?

But code is auto-generated anyway, and we can do everything. So I decided to leverage mixins concept. I've used <a href="http://mandango.org/doc/mondator/index.html">Mondator</a> (class generator for PHP), which also powers Doctrator, and now I can define extenstions without all this weird syntax above:

<pre>
namespace Model\Extension;

class Access {
  public function __call($method, $args) { ... }
  public function offsetExists($offset) { ... }
  public function offsetSet($offset, $value) { ... }
  public function offsetGet($offset) { ... }
  public function offsetUnset($offset) { ... }
}
</pre>

And then I inject them into entity via simple static method <code>mixins()</code> (anotations could be even neater here).

<pre>
namespace Model\Definition;

/**
 * @Entity
 * @Table(name="people")
 */
class Person {

  static public function mixins() {
    return array(
    	'Model\Extension\Access',
    );
  }

}
</pre>

Then, lib behind it just copies all methods and properties from classes (mixins) defined in <code>mixins()</code> method and incorporate them into entity. By defining it like this, it will be also quite easy to migrate to traits, once they are available.

<b>UPDATE (26.07):</b>
Doctrator has now templates, <a href="https://github.com/pablodip/doctrator/blob/twig/src/Doctrator/Extension/ActiveRecord/templates/activeRecord.php">see example</a>, so forget malicious hint above about Doctrator's ugly syntax. 

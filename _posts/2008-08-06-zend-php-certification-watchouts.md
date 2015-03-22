---
title: "Zend PHP Certification Watchouts"
---

<p>Over a year ago <a href="http://www.zend.com/store/education/certification/authenticate.php?ClientCandidateID=ZEND005043&RegistrationID=219445502">I've passed</a> <a href="http://www.zend.com/en/services/certification/                                                                     ">Zend PHP Certification exam</a>. Now I decided to prepare some notes for all the folks preparing to take the exam. It's simply list of things that could be tricky, even for advanced programmers. The list is definitely not complete, and none of these are exam questions (it's forbidden to disclose them) but I hope you will find it helpful anyway. </p>

<h3>PHP Basics</h3>

<p>Both types of single line comments, // and #, can be ended by ending the current PHP block using the PHP closing tag - ?>.

<pre>
# Example:
	// Do not show ?> this
# Output:
	this
</pre>

<p>The precision and range of integers and floats varies depending on the platform on which your scripts run.</p>

<p>Float data type is not always capable of representing numbers in the way you expect it to.

<pre>
# Example:
	echo (int) ((0.1 + 0.7) * 10);
# Output:
	7  // not 8, because the expression is stored internally as 7.999999
</pre>

<p>It is possible to create variables whose names do not folow the constraints.

<pre>
# Example:
	$name = '123';
	$$name = '456';
	echo ${'123'};
# Output:
	456
</pre>
	
<p>Although bitwise left shift (<<) and right shift (>>) can be used for a multiplication or a division by a power of two, they are not exactly the same - in particular, there are overflow and underflow scenarios that can yield unexpected results.

<pre>
# Example:
	$x = 1;
	echo $x << 32; 
	echo $x * pow(2,32);
# Output:
	0 // becuase all of the bits have been shifted out of the integer value
	4294967296 // correct value
</pre>
	
<p>Unlike what happens in many other languages, by-reference activity is often slower than its by-value counterpart, because PHP uses a clever "deffered-copy" mechanism that actually optimizes by-value assignments.</p>

<p>Remember always terminate a "break" statement with semicolon if it does not have any parameters. Otherwise you may end up causing the interpreter to randomly exit from more than one loop.</p>

<h3>Functions</h3>

<p>Functions can also be declared so that they return by reference. Typically, this is used for things like resources (like database connection) and when implementing Factory pattern.</p>

<p>You can pass to a function more arguments than you specified in its declaration.</p>

<p>Optional arguments can only take simple values - expressions are not allowed.</p>

<p>Unlike PHP 4, PHP 5 allows default values to be specified for parameters even when they are declared as by-reference.</p>

<p>Function body must be in block { }.</p>

<p>Function declarations can be split over multiple PHP segments.</p>

<h3>Arrays</h3>

<p>If not key is specified, PHP will automatically choose the next highest numeric key available or zero (0) if key is negative.</p>

<p>Array keys are case-sensitive, but type insensitive. Thus, the key "A" is different form the key "a", but the keys "1" and 1 are the same. However, the conversion is only applied if a string key contains the traditional decimal representation of a number (e.g. -2, 10); thus, for example, the key "01" is not the same as the key 1.</p>

<p>When using the addition operator + with arrays (it merges the elements) if the input arrays have the same keys, then the later value will be omitted.</p>

<p>The equivalence operator == returns true if both arrays have the same number of elements with the same values and keys, regardless their order. The identity operator ===, on the other hand, returns true only if the array contains the same key/value pairs in the same order.</p>

<p>The correct way to determine whether an array element exists is to use array_key_exists() for key and in_array() for value. Isset() has the major drawback of considering an element whose value is NULL as inexistent.</p>

<p>There is no correlation between the array pointer (element's actual position in the array) and the keys of the elements.</p>

<p>Using iterating by reference (foreach ($a as &$v)) is very tricky as it causes $v to be assigned by reference to each of the array's elements, so that, by the time the loop is over, $v is, in fact, a reference to last element of the array and it may lead to unexpected results later in the code.

<pre>
# Example:
	$a = array('zero', 'one', 'two');
	foreach ($a as &$v) {}
	foreach ($a as $v) {}
	print_r($a);
# Output:	
	Array ( [0] => zero [1] => one [2] => one )
</pre>

<p>Functions you should know really well: array_key_exists, in_array, array_flip, array_reverse, reset, key, next, prev, array_walk, array_walk_recursive, sort, asort, rsort, arsort, natsort, natcasesort, ksort, krsort, usort, uasort, uksort, shuffle, array_keys, array_push, array_pop, array_shift, array_unshift, array_diff, array_diff_assoc, array_diff_key, array_diff_uassoc, array_diff_ukey, array_intersect, array_intersect_key, array_intersect_assoc, array_intersect_ukey, array_intersect_uassoc.</p>

<h3>Strings and patterns</h3>

<p>String can be used as arrays - you can access the individual characters of a string as if they were members of an array.

<pre>
# Example:
	$string = 'abcdef';
	echo $string[1];
# Output:
	'b'
</pre>

<p>Functions you should know really well: strtr, strcmp, strcasecmp, strcasencmp, substr_compare, strpos, stripos, strrpos, strstr, stristr, strspn, strcspn, str_replace, str_ireplace, substr_replace, substr, setlocale, number_format, money_format, printf, sprintf, fprintf, sscanf, preg_match, preg_match_all, preg_replace</p>



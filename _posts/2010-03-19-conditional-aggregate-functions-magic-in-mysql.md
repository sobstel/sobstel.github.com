---
title: "Conditional aggregate functions magic in MySQL"
---

Probably the most common usage for <a href="http://dev.mysql.com/doc/refman/5.0/en/group-by-functions.html">aggregate functions</a> is to count all rows in the table using <code>COUNT(*)</code> combined with GROUP BY clause.

What you may not be aware of, is that this COUNT's asterisk is actually an expression. You probably even saw expression like COUNT(id) or SUM(amount), but there's available much more magic than that. You can put conditions there.

Let's say you have following table named <em>users</em>: 

<pre>
CREATE TABLE IF NOT EXISTS `users` (
  `id` int(11) NOT NULL auto_increment,
  `active` tinyint(1) NOT NULL,
  `premium` tinyint(1) NOT NULL,
  `balance` int(11) NOT NULL,
  `created_at` datetime NOT NULL,
  PRIMARY KEY  (`id`)
);
</pre>

You now have to get count of all activated users. Sure, it may be done by simple SELECT with WHERE condition. But what if you were about to get all activate users and all premium users? Another SELECT? What if you have more conditions? 

It can be done in one go, much faster and easier. Just use conditional aggregate functions:

<pre>
SELECT 
  COUNT( NULLIF(active, 0) ) active_users, 
  COUNT( NULLIF(premium, 0) ) premium_users
FROM users
</pre> 

Works like a charm. No need for another query, no need to combine results at server side, which is very convenient if you have much more complex queries that use joins and GROUP BY clause. Just simple expressions.

Another example. Let's say, you have to fetch both count and current balance for all active and premium users separately as well as count of inactive users, everything grouped by the year they signed up. A lot of data, at least 5 seperate queries you may think. No, look at this:

<pre>
SELECT 
  YEAR(created_at) sign_up_year,  
  COUNT( NULLIF(active, 0) ) active_users, 
  COUNT( NULLIF(premium, 0) ) premium_users,
  COUNT( NULLIF(active, 1) ) inactive_users,
  SUM( IF(active=1, balance, 0) ) active_users_balance, 
  SUM( IF(premium=1, balance, 0) ) premium_users_balance
FROM users
GROUP BY sign_up_year
</pre> 

Pretty magic, huh? Nice and powerful, and it actually saved a lot of my time recently. Not sure about its performance though, I haven't run any benchmarks, but it rather makes things faster than slower.

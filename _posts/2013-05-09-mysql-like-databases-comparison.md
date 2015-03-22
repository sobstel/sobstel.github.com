---
title: "Oracle MySQL 5.6 vs Percona Server 5.5 vs MariaDB 5.5"
---

### Database management systems overview

#### (Oracle) MySQL 5.6

The world's most popular open source database currently owned and driven by Oracle.

There were big optimizer improvements planned for MySQL 6.0, but it was eventually abandoned. MariaDB and Oracle teams have ported code from MySQL 6.0 to MariaDB 5.3 and MySQL 5.6 respectfully.

MySQL 5.6.x has brought great improvements in Innodb scalability, transparency and replication.  For example, multi-threaded slaves delivered significant performance increases to MySQL replication when handling multiple schemas.

First version of newest stable branch (5.6.x) has been released in April 2011. It’s a big leap - in terms of performance - in comparison with previous 5.5.x branch, so anything lower than 5.6 should not be even considered.

#### Percona Server 5.5

Percona Server is an enhanced drop-in replacement for MySQL. It offers breakthrough performance, scalability, features, and instrumentation.

Percona focus on providing a solution for the most demanding applications, empowering users to get the best performance and lowest downtime possible. Percona focuses on compatibility with (Oracle’s) MySQL, high quality, and small but high value changes focused on the most pressing problems they observe in the demanding environments their customers are running. 

Percona is known for numerous optimizations for InnoDB engine. They have developed XtraDB (drop-in replacement for InnoDB).

Unfortunately it's newest 5.6 branch is still not stable.

#### MariaDB 5.5

MariaDB is a community-developed fork of the MySQL led by its original developers (including Michael Widenius, the founder of MySQL) started after Oracle had taken over MySQL.

MariaDB 5.5 is compatible with MySQL 5.5. However next version is numbered 10 as it will not import all features from MySQL 5.6 breaking compatibility between these systems.

MariaDB's API and protocol are compatible with those used by MySQL, which means that all connectors, libraries and applications which work with MySQL should also work on MariaDB.

MariaDB is known of pushing boundaries with the most advanced optimizer in MySQL space as well as leading the way in integrating with NoSQL solutions with its Cassandra Storage Engine.

Worth mentioning is that upgrade from MySQL 5.0 to MariaDB is easier than from MySQL 5.0 to MySQL 5.1.

### Storage engines overview

#### InnoDB

InnoDB is the default storage engine for MySQL as of MySQL 5.5. It provides the standard ACID-compliant transaction features, along with foreign key support).

InnoDB became a product of Oracle Corporation after its acquisition of Innobase Oy in October 2005.

In contrast to MyISAM (previous default storage engine), it uses row-level locking, which means fewer lock conflicts when different sessions access different rows, fewer changes for rollbacks and possibility to lock a single row for a long time.

#### XtraDB

Percona XtraDB is an enhanced version of the InnoDB storage engine, designed to better scale on modern hardware, and including a variety of other features useful in high performance environments. It is fully backwards compatible, and so can be used as a drop-in replacement for standard InnoDB.

Percona XtraDB includes all of InnoDB's robust, reliable ACID-compliant design and advanced MVCC architecture, and builds on that solid foundation with more features, more tunability, more metrics, and more scalability.

It is designed to scale better on many cores, to use memory more efficiently, and to be more convenient and useful. The new features are especially designed to alleviate some of InnoDB's limitations.

Percona improvements include, among other things, CPU scalability fixes (improves performance on systems with multi-cpus), IO scalability fixes, better diagnostics, faster crash recovery, etc.

XtraDB is available as part of Percona Server and MariaDB only.

### Comparison matrix

<table class="mysql-comparison-matrix">
  <tr class="darker">
    <th>&nbsp;</th>
    <th>Oracle&nbsp;MySQL 5.6 (InnoDB)</th>
    <th>Percona&nbsp;Server 5.5 (InnoDB)</th>
    <th>MariaDB 5.5 (XtraDB)</th>
  </tr>
  <tr>
    <th>General information</th>
    <th>MySQL</th>
    <th>Percona</th>
    <th>MariaDB</th>
  </tr>
  <tr>
    <td>Latest stable release</td>
    <td>5.6.11 (April 2013)</td>
    <td>5.5.30 (April 2013)</td>
    <td>5.5.30 (March 2013)</td>
  </tr>
  <tr>
    <td>Future MySQL compatibility</td>
    <td>+</td>
    <td>+</td>
    <td>- (SQL syntax and startup options in 10.0, full in 10.1)</td>
  </tr>
  <tr>
    <td>MySQL data and table definition files (.frm) files binary compatibility</td>
    <td>+</td>
    <td>+</td>
    <td>+</td>
  </tr>
  <tr>
    <td>Client APIs, protocols and  connectors compatibility</td>
    <td>+</td>
    <td>+</td>
    <td>+</td>
  </tr>
  <tr>
    <th>InnoDB</th>
    <th>MySQL</th>
    <th>Percona</th>
    <th>MariaDB</th>
  </tr>
  <tr>
    <td>InnoDB compatibility</td>
    <td>+ (native InnoDB)</td>
    <td>+ (XtraDB)</td>
    <td>+ (XtraDB)</td>
  </tr>
  <tr>
    <td>Availability of XtraDB engine</td>
    <td>- (not available)</td>
    <td>+</td>
    <td>+</td>
  </tr>
  <tr>
    <td>InnoDB-specific improvements</td>
    <td>+</td>
    <td>- (only in XtraDB)</td>
    <td>- (only in 10.x)</td>
  </tr>
  <tr>
    <td>CPU scalability fixes (using multi-cpus more efficiently)</td>
    <td>+ (using operating system memory allocators)</td>
    <td>+ (XtraDB)</td>
    <td>+ (using jemalloc)</td>
  </tr>
  <tr>
    <td>I/O scalability fixes</td>
    <td>+ (not exactly the same as XtraDB)</td>
    <td>+ (XtraDB)</td>
    <td>+ (XtraDB)</td>
  </tr>
  <tr>
    <td>Crash recovery improvements</td>
    <td>+</td>
    <td>+ (XtraDB)</td>
    <td>+ (XtraDB)</td>
  </tr>
  <tr>
    <th>Replication</th>
    <th>MySQL</th>
    <th>Percona</th>
    <th>MariaDB</th>
  </tr>
  <tr>
    <td>Replication with multi-threaded slaves</td>
    <td>+ (per database)</td>
    <td>- (per database in 5.6 alpha)</td>
    <td>- (per table in 10.x</td>
  </tr>
  <tr>
    <td>Optimized row-based replication</td>
    <td>+</td>
    <td>n/a</td>
    <td>n/a</td>
  </tr>
  <tr>
    <td>Multi-source replication</td>
    <td>-</td>
    <td>-</td>
    <td>- (only in 10.x)</td>
  </tr>
  <tr>
    <th>Others</th>
    <th>MySQL</th>
    <th>Percona</th>
    <th>MariaDB</th>
  </tr>
  <tr>
    <td>Optimizer enhancements</td>
    <td>+</td>
    <td>- (in 5.6)?</td>
    <td>+ (probably best of all)</td>
  </tr>
  <tr>
    <td>NoSQL Interface via memcached</td>
    <td>+ (memcached)</td>
    <td>-</td>
    <td>+ (cassandra, memcached in 10.x)</td>
  </tr>
  <tr>
    <td>File Sort Optimization</td>
    <td>+</td>
    <td>- (in 5.6)?</td>
    <td>- (only in 10.x)</td>
  </tr>
  <tr>
    <td>Faster ORDER BY nidxcol LIMIT N</td>
    <td>+</td>
    <td>- (in 5.6)?</td>
    <td>- (in 10.x)</td>
  </tr>
  <tr>
    <td>JOIN optimizations</td>
    <td>-</td>
    <td>-</td>
    <td>+</td>
  </tr>
  <tr>
    <td>EXPLAIN  improvements</td>
    <td>+</td>
    <td>- (in 5.6)?</td>
    <td>- (partly, fully in 10.0.2)</td>
  </tr>
  <tr>
    <td>Disk access optimizations</td>
    <td>+</td>
    <td>- (in 5.6)?</td>
    <td>+ (more than MySQL)</td>
  </tr>
  <tr>
    <td>Cost-based choice of range vs. index_merge</td>
    <td>-</td>
    <td>-</td>
    <td>+</td>
  </tr>
  <tr>
    <td>Subquery optimizations</td>
    <td>+</td>
    <td>- (in 5.6)?</td>
    <td>+</td>
  </tr>
</table>


### Summary (pros and cons)

#### Oracle MySQL 5.6

Pros

* Huge number of important optimizations and improvements between 5.5 and 5.6 branches.
* Multi-threaded replication
* Faster ORDER BY nidxcol LIMIT N.
* File sort optimizations.
* EXPLAIN improvements.

Cons

* No XtraDB (although there are many InnoDB optimizations introduced in 5.6 branch)
* No join optimizations (that MariaDB introduced)
* Likely to be closed (not open-source anymore)

#### Percona Server 5.5.

Pros

* XtraDB
* Fully compatible with MySQL (also in the future)

Cons

* No support for multi-threaded replication.
* 5.6 branch is not stable, and not 5.5 misses many optimizations (introduced to MySQL 5.6 and MariaDB 5.5).

#### MariaDB 5.5

Pros

* XtraDB
* More advanced join and disk access optimizations
* Better upgrade scripts (from MySQL) than MySQL itself

Cons

* No support for multi-threaded replication
* No optimizations for ORDER BY nidxcol LIMIT N
* New version incompatible with MySQL (so no way back)

### Update (03-09-2013): Feedback from Michael Widenius (MariaDB)

* MariaDB 10.0 should have the full SQL syntax and be able to handle all startup options that MySQL 5.6 has. However some startup options that only affects timing may not be implemented same way (in 10.0 they'll be re-engineered).
* MariaDB 10.1 will be fully compatible with MySQL 5.6. In effect this means that almost all user will be able to move from MySQL 5.6 to MariaDB 10.0 without any other issues than that some things are faster and other things are slower.
* MariaDB 10.0 has a LOT of optimization that MySQL 5.6 doesn't have.
* There are tons of the features that are unique to MariaDB, eg EXPLAIN on a running query (all listed at [kb.askmonty](https://kb.askmonty.org/en/mariadb-vs-mysql-features/)).
* For a true comparison, one should also not forget the most important feature in 5.6, global transaction id, [is not really working](https://lists.launchpad.net/maria-developers/msg04837.html).
* MariaDB has everything that Percona has in 5.5.

### Update (05-09-2013): Feedback from Michael Widenius (MariaDB)

* 10.0 will have all the syntax and options and 5.6 but different performance (in some case faster, in some case slower). 10.1 should almost always be faster than MySQL 5.6.
* One database is replicated serial in 5.6 and in parallel in 10.0. In very simple tests there was a 4X speedup for the new (MariaDB) replication when updating the same table.
* Michael disagrees that 5.6 is a stable solution (especially if you try to use the new replication features).

### References

* [Percona: MySQL 5.6: Advantages in a Nutshell](http://www.percona.com/sites/default/files/presentations/Webinar-Mar2013-MySQL-5%206-Advantages-In-the-Nutshell.pdf)
* [MySQL: What's New in MySQL 5.6](https://dev.mysql.com/tech-resources/articles/whats-new-in-mysql-5.6.html)
* [Oracle's MySQL Blog: Benchmarking MySQL Replication with Multi-Threaded Slaves](https://blogs.oracle.com/MySQL/entry/benchmarking_mysql_replication_with_multi)
* [AskMonty: Upgrading to MariaDB from MySQL 5.0 (or older version)](https://kb.askmonty.org/en/upgrading-to-mariadb-from-mysql/)
* [MySQL Performance Blog: MySQL 5.6: Improvements in the Nutshell
](http://www.mysqlperformanceblog.com/2013/01/27/mysql-5-6-improvements-in-the-nutshell/)
* [MySQL Performance Blog: XtraDB: The Top 10 enhancements](http://www.mysqlperformanceblog.com/2009/08/13/xtradb-the-top-10-enhancements/)
* [MySQL Performance Blog: MySQL Limitations Part 1: Single-Threaded Replication](http://www.mysqlperformanceblog.com/2010/10/20/mysql-limitations-part-1-single-threaded-replication/)
* [SkySQL: MySQL 5.6 vs MariaDB 10.0: A high-level comparative overview of the features](http://www.skysql.com/blogs/max-mether/mysql-56-vs-mariadb-100)
* [AskMonty: Optimizer Feature Comparison Matrix](https://kb.askmonty.org/en/optimizer-feature-comparison-matrix)








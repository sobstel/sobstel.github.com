---
title: "Random MySQL optimization tips"
---

Data types
----------

* Use small data types...
  * eg. `VARCHAR(20)` and not `VARCHAR(255)`` or `SMALLINT` and not `BIGINT`
  * ...so more records fit in a single page of memory. Faster seeks and scans.
* Use appropriate data types...
    * `INT UNSIGNED` for IP addresses
    * Avoid `TEXT` and `BLOB`. Consider separate tables or use the filesystem.
        * They result in using a disk. Slow.

Indexes
-------

* It's no silver bullet.
** Might speed up SELECTs, but slow down INSERTs/UPDATEs.
* Ensure JOIN conditions are indexed and have identical data types.
* Consider indexes on columns used in WHERE and GROUP BY clauses.
* Column order matters.
* Check index selectivity.
** % of distinct values in a column.
*** Unique/primary always is 1.0.
** For low-selectivity columns, full scan is usually more efficient.
* Leverage covering indexes.
** When all columns from a given table are available in the index.
   Data fetched directly from index without table lookup. Fast.

Too many joins
--------------

* For small, static lookups, use ENUM (or SET for many-to-many).

Partitioning
------------

* Vertical (split table with many columns into multiple tables)
** When extra columns are mostly NULL
** When extra columns are infrequently used
** Need FULLTEXT on your text columns
* Hortizontal (split table with many rows into multiple tables)
** Sharding?

Configuration
-------------

* `table_open_cache` - numbe of simultaneously open file descriptors
  (more joins, higher it should be)
* `innodb_buffer_pool_size`
** if InnoDB-only system, set to 60-80% of total memory available
** watch for `Innodb_buffer_pool_pages_free` approaching 0
* `innodb_log_file_size`
** set to 40-50% of `innodb_buffer_pool_size`
** bigger means longer recovery, but less disk operations due to less
   checkpoint flush activity
* `innodb_log_buffer_size`
** set to <16M (recommended 1M to 8M)
* `innodb_flush_method`
** if getting lots of `innodb_data_pending_fsyncs` consider O_DIRECT
* examine `Innodb_buffer_pool_reads` vs `Innodb_buffer_pool_read_requests`
  for the cache hit ratio
* examine `Qcache_hits`/`Questions` for the query cache hit ratio
* ensure `Qcache_lowmem_prunes` is low
* ensure `Qcache_free_blocks = 1`, if not `FLUSH QUERY CACHE`

Tools
-----

* [EXPLAIN cheatsheet (pdf)](https://www.pythian.com/blog/wp-content/uploads/explain-diagram1.pdf)

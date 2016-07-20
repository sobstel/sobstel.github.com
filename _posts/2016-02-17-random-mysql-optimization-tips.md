---
title: "Random MySQL optimization tips"
---

SELECTs
-------

* Select only what you need. Avoid `SELECT *`.
  * More columns, less fit into a memory, bigger probability it'll be dumped to disk. Slow.
* Know [access strategies](http://joinfu.com/presentations/target-practice/target-practice-workbook.pdf)

EXPLAIN
-------

* Use [EXPLAIN](https://www.pythian.com/blog/wp-content/uploads/explain-diagram1.pdf) to find query issues.
* `Using filesort` has nothing to do with files. Name is confusing. It just means that optimizer
  needed to sort data (might be prevented with having properly ordered index, which is hard to do for most
  cases).
* `Using temporary` refers to temporary in-memory table


Data types
----------

* Use small data types...
  * eg. `VARCHAR(20)` and not `VARCHAR(255)` or `SMALLINT` and not `BIGINT`
  * ...so more records fit in a single page of memory. Faster seeks and scans.
* Use appropriate data types...
  * `INT UNSIGNED` for IP addresses
  * Avoid `TEXT` and `BLOB`. Consider separate tables or use the filesystem.
    * They result in using a disk. Slow.
* For small, static lookups, use ENUM (or SET for many-to-many).

Indexes
-------

* It's no silver bullet.
  * Might speed up SELECTs, but slow down INSERTs/UPDATEs.
* Ensure JOIN conditions are indexed and have identical data types.
* Consider indexes on columns used in WHERE and GROUP BY clauses.
* Column order matters.
* Check index selectivity.
  * % of distinct values in a column.
    * Unique/primary always is 1.0.
  * For low-selectivity columns, full scan is usually more efficient.
* Leverage covering indexes.
  * When all columns from a given table are available in the index.
   Data fetched directly from index without table lookup. Fast.
* `Created_tmp_tables` increasing dramatically typically means a lack
  of necessary index for a frequently executed query

Subqueries
----------

* Correlated subqueries are evil.
  * They're executed over and over again for each matched row.
  * Use `EXPLAIN` (`DEPENDENT SUBQUERY`) and
    [SHOW PROFILE](http://www.sobstel.org/blog/mysql-profiling-queries/)
    to detect this.

Partitioning
------------

* Vertical (split table with many columns into multiple tables)
  * When extra columns are mostly NULL
  * When extra columns are infrequently used
  * Need FULLTEXT on your text columns
* Horizontal (split table with many rows into multiple tables)
  * Sharding?

Configuration
-------------

* `long_query_time` and `log-slow-queries` to determine if you need to
   optimize in a first place
* `table_open_cache` - number of simultaneously open file descriptors
  (more joins, higher it should be)
* `max_heap_table_size` / `tmp_table_size`
  * increase them when `Created_tmp_disk_tables` counter increases dramatically
* `innodb_buffer_pool_size`
  * if InnoDB-only system, set to 60-80% of total memory available
  * watch for `Innodb_buffer_pool_pages_free` approaching 0
* `innodb_log_file_size`
  * set to 40-50% of `innodb_buffer_pool_size`
  * bigger means longer recovery, but less disk operations due to less
   checkpoint flush activity
* `innodb_log_buffer_size`
  * set to <16M (recommended 1M to 8M)
* `innodb_flush_method`
  * if getting lots of `innodb_data_pending_fsyncs` consider O_DIRECT
* examine `Innodb_buffer_pool_reads` vs `Innodb_buffer_pool_read_requests`
  for the cache hit ratio
* examine `Qcache_hits`/`Questions` for the query cache hit ratio
* ensure `Qcache_lowmem_prunes` is low
* ensure `Qcache_free_blocks = 1`, if not `FLUSH QUERY CACHE`

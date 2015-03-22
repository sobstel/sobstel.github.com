---
title: "MySQL profiling queries"
---

### Usage

<pre>
SET profiling = 1;
SELECT ...;
SHOW PROFILE;
</pre>

or

<pre>
SET profiling = 1;
SELECT ...;
SELECT ...;
SELECT ...;
SHOW PROFILES;
SHOW PROFILE FOR QUERY 1;
</pre>

### Sample output

<pre>
mysql> show profile;
+-+
| Status               | Duration |
+-+
| starting             | 0.000135 |
| checking permissions | 0.000004 |
| checking permissions | 0.000003 |
| checking permissions | 0.000002 |
| checking permissions | 0.000002 |
| checking permissions | 0.000002 |
| checking permissions | 0.000002 |
| checking permissions | 0.000002 |
| checking permissions | 0.000003 |
| checking permissions | 0.000002 |
| checking permissions | 0.000004 |
| Opening tables       | 0.000043 |
| init                 | 0.000089 |
| System lock          | 0.000013 |
| optimizing           | 0.000034 |
| statistics           | 0.000285 |
| preparing            | 0.000043 |
| Creating tmp table   | 0.000042 |
| Creating tmp table   | 0.000032 |
| Sorting result       | 0.000005 |
| executing            | 0.000003 |
| Sending data         | 1.394402 |
| Creating sort index  | 0.000025 |
| Creating sort index  | 0.000013 |
| end                  | 0.000004 |
| removing tmp table   | 0.000007 |
| end                  | 0.000002 |
| removing tmp table   | 0.000003 |
| end                  | 0.000006 |
| removing tmp table   | 0.000004 |
| end                  | 0.000003 |
| query end            | 0.000007 |
| closing tables       | 0.000021 |
| freeing items        | 0.000016 |
| removing tmp table   | 0.000004 |
| freeing items        | 0.000014 |
| cleaning up          | 0.000013 |
+-+
37 rows in set, 1 warning (0.01 sec)
</pre>

More details: [SHOW PROFILE Syntax](http://dev.mysql.com/doc/refman/5.6/en/performance-schema-configuration.html).
---
title: "MySQL DATETIME vs TIMESTAMP"
---

`DATETIME` 

  * uses 8 bytes of storage,
  * ranges from the year 1001 to 9999,
  * stores the value as integer,
  * independent of time zone.
  
`TIMESTAMP` 

  * uses 4 bytes of storage,
  * ranges from they year 1970 to (part of) 2038,
  * depends on the time zone settings, 
  * NOT NULL by default,
  * the first TIMESTAMP column is set to the current time when row is inserted/updated without specifying a value for the column.

Both are displayed according to YYYY-MM-DD HH:MM:SS format.

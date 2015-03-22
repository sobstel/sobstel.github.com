---
title: "From Relational to NoSQL in a nutshell"
---

Relational vs NoSQL wars are pointless. There's just no one silver bullet to solve all the problems. It's a truism that too many forget far too often. What's more, both SQL databases and NoSQLs can successfully co-exist.

### Why migrate to NoSQL?

Go into NoSQL if you require high availability more than anything else (at cost of immediate consistency).
Relational databases sacrifice availability for consistency and master database is usually a single point of failure.

Famous [CAP theorem](http://en.wikipedia.org/wiki/CAP_theorem) states that in the event of a network partition, a distributed system can either provide availability (it's NoSQL) or consistency (it's Relational).

It doesn't mean NoSQL databases are not consistent at all, they're just [eventually consistent](http://en.wikipedia.org/wiki/Eventual_consistency).

### Cost of Scale

Scaling a relational database to handle more traffic is usually painful experience for ops. It's definitely neither cheap nor easy. Most NoSQLs are designed with scaling in mind.

### Data model

Relational databases are strict, which makes it very hard to store unstructured data. NoSQL are much less conservative about it.
They're usually schemaless. It also often means that adding new features does not require costly schema changes.

### Tradeoffs

* No strict consistency in NoSQLs.
* Data modelling. Poor data types support in NoSQLs.
* MapReduce in NoSQL. SQL is often much more expressive and easier to write.
* Less query options in NoSQLs. SQL is really great language.
* Very poor JOIN-like support. Well, it's NoSQL after all! On the other hand, join decomposition is one of best ways to optimize SQL queries.
* No transactions. If you need ACID, you usually should pick SQL db.

This has been all compiled based on great "[From Relational to Riak](http://basho.com/assets/RelationaltoRiakDEC.pdf)" document.

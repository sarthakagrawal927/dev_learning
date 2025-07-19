# Databases

## ACID

A - Atomicity - Does this query happen all at once? You cannot divide the transaction.
C - Consistency - Making sure multiple instances of the db are on the same page.
I - Isolation - You can divide the query into multiple parts and run them parallel/ serially and result will be the same.
D - Durability - Restoration in case of crashes.

## Transactions

Some queries need multiple things to happen at once, and you need to make sure that either all things happen or none.

## Non-Relational

- Schema-less - You can design it on the fly - with great power comes great responsibility - if you mis-spell any field in the query it will just create another field.

### MongoDB

- When we are indexing in MongoDB, it actually creates a tree in memory, thus instead of looking through the complete list, it goes through by tree. In mongoDB, _id is indexed by default and we can index whatever column we want based on the frequency of queries of that columns ( even nested ) - don't have useless indexes. To make anything unique - you will have to index it.

- FullTextSearch - You can have only one text index, in which you can search all fields (that you have indexed) via text, MongoDB will even provide you with matching score for each result with you can sort and send - not very useful since elasticSearch came.

- Aggregation Pipeline ( evolved from mapReduce ) - to deliver insights (analytics) from your databases.

### Neo4J

Graph databases, extremely useful for building recommendation systems & social networks.

### Redis

Key-Value storage - very fast & scalable - very large JS Object. Caching, telemetry.
Study HyperLogLog (like Bloom Filter), Streams (like pub-sub).

## Relational

- Very structured schema, you can shut off the server trying to run an alter schema query. Data relates to each other. SQL is structured query language to query the databases.

### PostgreSQL

Very flexible & Scalable. Use explain before query to understand the query plan. Creating index is the way to go for faster queries. Can have JSON as value in a column.

[Mysql Internals](https://enhancedformysql.github.io/The-Art-of-Problem-Solving-in-Software-Engineering_How-to-Make-MySQL-Better)

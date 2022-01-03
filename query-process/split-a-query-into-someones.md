# Query splitting

### Overview

Query-frontend receives a query request and splits them into someones.

The reason why it splits a query is because it increases the query parallelism and makes more queriers process in parallel. This gives us a performance improvement.

There are two main strategies to split as listed.

* Split by time range
* Split by shard number

### Split by time range

You can set this setting as "split\_queries\_by\_interval" parameter.

Let's imagine that this parameter is 15m, which means that a query is split by 15m.

For example, a query that searches logs for an hour is split into four queries due to the parameter.

That's how we can process the query in parallel.

### Split by shard number

In the query process, the queriers use inverted indexes to search logs.

The inverted indexes are split by shard number in advance in the ingestion process.

Therefore, in the query process, query-frontend splits a query into someones and automatically adds a shard number in each query so that the query is processed by multiple queriers in parallel.

![](../.gitbook/assets/query-process-split-logql-by-shard.png)

It means that the matched chunks that are determined in each querier are fewer than before splitting.

Please read the further section to understand more detail about the query processing in parallel.

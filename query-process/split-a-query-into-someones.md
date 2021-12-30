# Query splitting

### Overview

Query-frontend receives a query request and then splits them into someones.

The reason why it splits a query is that it increases the query parallelism and makes more queriers process in parallel. It gives you a performance improvement.

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

Therefore, in the query process, query-frontend splits a query by shard count and automatically adds shard number in each query so that the query is processed by multiple queriers in parallel.

![](<../.gitbook/assets/スクリーンショット 2021-12-23 21.25.18.png>)

It means that the target chunks that are processed in each querier are fewer than before splitting.

Please read the further section to understand more detail about the query processing in parallel.

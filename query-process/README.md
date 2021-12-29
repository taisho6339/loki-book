# Query Process

### Overview

In this chapter, I'll introduce the query process of Loki to you.

You can get the following understandings here.

* The components to query
* How to split queries
* How to schedule query
* How to use inverted index
* How to aggregate the results from queriers

### Components for query

![](<../.gitbook/assets/スクリーンショット 2021-12-23 19.49.54.png>)

Here are the components to query logs.

#### Ingesters

Ingesters are here to be queried for unflushed logs.

#### Query Frontend

This is an optional component but very important for query performance.

It is a proxy for queriers. Here are its jobs.

* It splits queries and schedules them to some queriers and aggregates the results from them
* It retries query requests if they fail
* It caches query results
* It manages rate limit

Especially processing some queries in parallel gives us dramatic improvement in performance.

#### Querier

Querier is an actual query processor.

#### AWS S3 as Storage Engine

The logs are stored in this persistently.

This layer also supports Cassandra, GCS, DynamoDB, and other products as storage.

Here are the supported stores.

[Supported Stores](https://grafana.com/docs/loki/latest/operations/storage/)

#### Memcached as Cache

It is an optional component but very effective for query performance.

There are four types of cache in Loki.

You can know more details about it.

{% content-ref url="../cache-strategy.md" %}
[cache-strategy.md](../cache-strategy.md)
{% endcontent-ref %}

### Read Path

Here is the Read Path in Loki.

1. A query-frontend receives a query request and splits them into someones
2. The query-frontend enqueue some queries and some queriers get some queries from the queue
3. A querier requests for all of the ingesters to select unflushed logs
4. The querier gets the target chunks using inverted indexes in index cache or BoltDB
5. The querier downloads the chunks from the chunk cache or AWS S3 and filters them
6. The query-frontend aggregates all of the results, sorts, removes duplicates, and then returns the result

In further sections, I'll give you more detailed mechanisms.

# Cache Strategy

### Overview

Loki supports four kinds of cache for performance improvement.

Here are the caches.

* Index read cache
* Index write cache
* Chunk cache
* Query result cache

### Index read cache

Index-read-cache caches inverted indexes.

Queriers use this cache before they ask BoltDB or Index Gateway for them.

In addition, the inverted indexed will be cached in this cache only when they are missed there in query processes.

### Index write cache

Index-write-cache is used to avoid duplicate writing indexes.

Ingesters use this cache and the indexes are written on it when flushing chunks.

Please read this link if you want to know more detail.

{% embed url="https://cortexmetrics.io/docs/chunks-storage/caching#index-write-cache" %}

However, if we use BoltDB-Shipper, this cache shouldn't be used because of this reason.

{% embed url="https://grafana.com/docs/loki/latest/operations/storage/boltdb-shipper#write-deduplication-disabled" %}

Alternatively, we should use Compactor to reduce the duplications.

{% embed url="https://grafana.com/docs/loki/latest/operations/storage/boltdb-shipper#compactor" %}

### Chunk cache

Chunk cache stores log chunks, which are encoded like persistent storage.

Ingesters and queriers use this cache.

The ingesters write chunks to persistent storage like AWS S3 and then, write them back to this cache, so it's a write-through cache.

It is also used to reduce duplicate writing chunks in ingesters.

The ingesters check if the chunk key already exists before writing and if exists, they will stop.

Typically, memcached is used for that but it doesn't like to receive chunks that are more than 500KiB so the writing to this cache can fail frequently.

It means that it can make query performance worse.

### Query result cache

Query result cache stores query results and query-frontends use this cache.

This link helps you to understand.

{% embed url="https://cortexmetrics.io/docs/chunks-storage/caching#query-results-cache" %}

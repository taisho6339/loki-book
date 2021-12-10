# Ingestion Process

### Overview

In this section, I'll introduce the ingestion process of Loki to you.

You can get the following understandings.

* The components to ingest
* Ingestion mechanism
* Log chunk buffering & flushing
* Write-Ahead Log mechanism
* Unordered logs and Orderered logs

### Components for ingestion

![Ingestion Overview](.gitbook/assets/loki-book-ingestion.drawio.png)

Here are the components to ingest logs.

Fluentd, Fluentd Bit, Logstash, and Pomtail are the clients for Loki, and Distributor and Ingester are Loki's components.

In addition, AWS S3 is a chunk and index storage and Memcache is a cache layer for them.

Let me introduce them in order.

#### Clients for Loki

These components collect logs from your applications and send them to Loki via Distributor's HTTP endpoint.

You can build your own client because of sending via the API.

#### Distributor

This component has the responsibility to validate the ingestion request and distribute the requests to appropriate ingesters.

#### Ingester

This component has the responsibility to post logs to the storage engine like S3 and cache them to the chunk cache. (Memcached, Redis, etc)

#### AWS S3 as Storage Engine

The logs are stored in this persistently.

This layer also supports Cassandra, GCS, DynamoDB, and so on... as the storage.

Here are the supported stores.

[Supported Stores](https://grafana.com/docs/loki/latest/operations/storage/)

#### Memcached as Cache

It is an optional component but very effective for query performance.

There are four types of cache in Loki.

You can know more details about it.

// TODO:&#x20;

[Loki's Cache](ingestion-process.md#overview)

### Requests from Clients to Distributors

![](<.gitbook/assets/スクリーンショット 2021-12-03 10.32.47.png>)

Loki clients construct log bodies and TenantID as HTTP requests.

Let's dive into the requests.

When it comes to TenantID, the clients annotate the ID to HTTP Header so that Loki can recognize which tenant of logs it receives.

About the request body, it has 3 types of fields, stream, timestamp, and actual log body.

Here is the actual format.

```
{
  "streams": [
    {
      "stream": {
        "label": "value"
      },
      "values": [
          [ "<unix epoch in nanoseconds>", "<log line>" ],
          [ "<unix epoch in nanoseconds>", "<log line>" ]
      ]
    }
  ]
}
```

#### What is Stream?

The logs should have some labels like Prometheus.

We call the unique pattern that is a combination of TenantID and label key-values "Stream".

The logs are going to be chunked and managed in every stream.

#### Validation

The requests from the clients are validated by Distributors.

What points does it validate?

Mainly, it checks the following.

* Label format
  * invalid format?
* Log Body
  * too long?
* Log timestamp
  * too future or too old?
* Rate limit

If the rate limit mode is 'global', distributors are clustered so that they can know the number of healthy instances in the cluster and calculate the ingestion rate for all over that.

It means that distributors are clustered for the validation.

// TODO:

If you want to know more detail about the clustering, please see here.&#x20;

### Requests from Distributors to Ingesters

Ingesters are clustered as well and distributors can know which instances are healthy.

They have their own tokens and distributors distribute traffics to some of them by using "Consistent Hash" algorithm.

How does it route?

At first, it generates the hash value from tenantID and stream(label key values).

![](<.gitbook/assets/スクリーンショット 2021-12-08 9.33.56.png>)

And then, the Distributor requires the matched Ingesters with the stream hash.

Once the stream token matches with an ingester, in addition, it requires more for the replication factor in a clockwise direction.

It means that the distributor sends logs to some ingesters in duplicate to replicate them.

Finally, it regards the request as successful if more ingesters than half return 200 status.

![](<.gitbook/assets/スクリーンショット 2021-12-08 9.38.03.png>)

### Ingester Request Handling

Let's see how it handles the requests.

At first,  coming logs are appended to "Memory Chunk" by each stream.

![](<.gitbook/assets/スクリーンショット 2021-12-10 9.35.44.png>)

In addition, the write-ahead logs for each log are written on Ingester's disk to prevent it from losing log data unexpectedly. (described later)

Lastly, if the appending is succeeded, ingester returns success as gRPC response.

### Chunk Buffering

The chunks aren't stored in remote storage like AWS S3 immediately.

They are buffered unless they satisfy the conditions and after that flushed to remote storage.

The memory chunk is structured like this image.

![](<.gitbook/assets/スクリーンショット 2021-12-10 10.04.15 (1).png>)

It has an array called "Head" and another one called "Blocks".

At first, incoming logs are appended to "Head" with keeping the raw data.

When its size reaches "chunk\__block_\_size", Ingester compresses the all of elements in "Head" into a block.

If the total size of the memory chunk reaches "chunk\__target\__size", Ingester makes it read-only mode and appends it to a flush queue.

Does buffering logs mean that we can't query for recent logs?

No, it doesn't. Ingesters are also queried for logs from queriers.

So, How does it search all over memory chunks in Ingesters efficiently?

The answer is using inverted indexes.

Ingester has inverted indexes in their memory like the following image.

![](.gitbook/assets/memory\_inverted\_index.png)

It has map structures that map label values and fingerprints for each label name.

In addition, the fingerprints are generated as hash values of label key-value pairs for each stream and Ingester also has map data that maps fingerprints to actual streams.

Here is the flow of how Ingester extracts the matched chunks from memory.

1. Select the matched fingerprints for each label value
2. Extract intersecting fingerprints across all of them
3. Resolve the matched streams and chunks in those from the fingerprints

### Flushing Chunks



### Write-Ahead Log










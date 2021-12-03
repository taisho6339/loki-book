# Ingestion Process

### Overview

In this section, I'll introduce the ingestion process of Loki to you.

You can get the following understandings.

* The components to ingest
* Ingestion mechanism
* Log chunk buffering & flushing
* Write Ahead Log mechanism
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










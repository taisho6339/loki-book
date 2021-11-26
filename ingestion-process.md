# Ingestion Process

### Overview

In this section, I'll introduce the ingestion process of Loki to you.

You can get the following understandings.

* The components to ingest
* Ingestion mechanism
* Log chunk buffering & flushing
* Write Ahead Log mechanism
* Unordered logs and Orderered logs

### Components to ingest

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

This component&#x20;


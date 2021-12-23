# Query Process

### Overview

In this chapter, I'll introduce the query process of Loki to you.

You can get the following understandings here.

* The components to query
* How to split queries
* How to schedule query
* How to use inverted index
* How to aggregate the results from queriers
* What is boltdb-shipper and how it works

### Components for query

![](<../.gitbook/assets/スクリーンショット 2021-12-23 19.49.54.png>)

Here are the components to query logs.

#### Ingesters

Ingesters are here to be queried for unflushed logs.

#### Query Frontend

Query Frontend splits queries and schedules them to some queriers and aggregates the results from them.

#### Querier

Querier is an actual query processor.

#### AWS S3

AWS S3 is the storage for chunks and inverted indexes.

In addition, there are three kinds of cache and they are all Memcached here. (But we can use Redis or FIFO cache)

#### Query Path








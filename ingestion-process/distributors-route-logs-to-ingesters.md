# Routing logs to Ingesters

### Overview

Ingesters are clustered as well as distributors so distributors can know which instances are healthy.

Each ingester instance has its own token and distributors route traffics to some of healthy them by using the consistent-hash algorithm.

### How does it route?

At first, it generates the hash value from the stream.

![](../.gitbook/assets/ingestion-process-generate-hash-tenant-stream.png)

And then, the distributor requires the matched ingester instances with the generated hash value.

It doesn't require only an instance but also the replication factor number of instances in a clockwise direction in the hash ring for replication.

It means that the distributor sends logs to some ingesters in duplicate to replicate them.

Finally, it regards the request as successful if more the got ingesters than half return HTTP 200 status.

![](../.gitbook/assets/ingestion-process-request-ingesters.png)

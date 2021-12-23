# 2. Requests from Distributors to Ingesters

Ingesters are clustered as well and distributors can know which instances are healthy.

They have their own tokens and distributors distribute traffics to some of them by using "Consistent Hash" algorithm.

How does it route?

At first, it generates the hash value from tenantID and stream(label key values).

![](<../.gitbook/assets/image (5).png>)

And then, the Distributor requires the matched Ingesters with the stream hash.

Once the stream token matches with an ingester, in addition, it requires more for the replication factor in a clockwise direction.

It means that the distributor sends logs to some ingesters in duplicate to replicate them.

Finally, it regards the request as successful if more ingesters than half return 200 status.

![](<../.gitbook/assets/image (8).png>)

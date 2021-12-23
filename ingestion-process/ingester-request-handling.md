# Ingester Request Handling

Let's see how it handles the requests in ingesters.

At first, coming logs are appended to "Memory Chunk" by each stream.

![](<../.gitbook/assets/image (4).png>)

In addition, the write-ahead logs for each log are written on Ingester's disk to prevent it from losing log data unexpectedly. (described later)

Lastly, if the appending is succeeded, ingester returns success as gRPC response.

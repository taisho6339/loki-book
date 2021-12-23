# Chunk Buffering

The chunks aren't stored in remote storage like AWS S3 immediately.

They are buffered unless they satisfy the conditions and after that flushed to remote storage.

The memory chunk is structured like this image.

![](<../.gitbook/assets/image (1).png>)

It has an array called "Head" and another one called "Blocks".

At first, incoming logs are appended to "Head" with keeping the raw data.

When its size reaches "chunk\__block_\_size", Ingester compresses the all of elements in "Head" into a block.

If the total size of the memory chunk reaches "chunk\__target\__size", Ingester makes it read-only mode and appends it to a flush queue.

Does buffering logs mean that we can't query for recent logs?

No, it doesn't. Ingesters are also queried for logs from queriers.

So, How does it search all over memory chunks in Ingesters efficiently?

The answer is using inverted indexes.

Ingester has inverted indexes in their memory like the following image.

![](<../.gitbook/assets/image (3).png>)

It has map structures that map label values and fingerprints for each label name.

In addition, the fingerprints are generated as hash values of label key-value pairs for each stream and Ingester also has map data that maps fingerprints to actual streams.

Here is the flow of how Ingester extracts the matched chunks from memory.

1. Select the matched fingerprints for each label value
2. Extract intersecting fingerprints across all of them
3. Resolve the matched streams and chunks in those from the fingerprints


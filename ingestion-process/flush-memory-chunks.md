# Flushing memory chunks

### When the chunks are flushed to AWS S3?

There are main 4 conditions for that.

* Reach \`chunk\_target\_size\`
* Reach \`chunk\_idle\_period\` after the latest appending
* Reach \`chunk\_max\_age\` after the creation
* Force flush (eg. flush at process shutdown

Of course, they aren't flushed immediately when they match any of the conditions.

They are enqueued to a flush queue and flushed in order.

Here is the design for flushing.

![](<../.gitbook/assets/ingestion-process-flush-design.png>)

A goroutine observes all of the streams and checks if the chunks in each stream match the flush conditions. If some chunks match them, it enqueues them for flushing.

Another goroutine observes the flush queue. It takes a chunk from the queue and flushes it to persistent storage.

There are 3 steps in the flow.

1. Create an inverted index entry for each chunk and add it to BoltDB on Ingester's disk
2. Post the chunk to S3
3. Write it back to chunk cache

Of course, the inverted index is different from the memory chunk.

This is described later chapter, Query Process.

#### Avoid flushing chunks in duplicate

Distributors route logs to multiple ingesters so it can cause chunk duplication.

The ingester doesn't flush a chunk if the chunk is already cached in the chunk cache so that it avoids flushing duplicate chunks.

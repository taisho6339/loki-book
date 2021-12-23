# 5. Flushing Chunks

When the chunks are flushed to AWS S3?

There are main 4 conditions for that.

* Reach \`chunk\_target\_size\`
* Reach \`chunk\_idle\_period\` after the latest appending
* Reach \`chunk\_max\_age\` after the creation
* Force flush (eg. flush at process shutdown

Of course, they aren't flushed immediately when match any of the conditions.

They are enqueued to a flush queue and flushed in order.

Here is the design for flushing.

![](<../.gitbook/assets/image (6).png>)

A goroutine observes all of the streams and checks if the chunks in each stream match the flush conditions. If some chunks match them, it enqueues them for flushing.

Another goroutine observes the flush queue. It takes a chunk from the queue and flushes it to persistent storage.

There are 3 steps in the flow.

1. Create an inverted index entry for each chunk and Add to BoltDB on Ingester's disk
2. Post the chunk to S3
3. Write back the chunk to Memcache, it is managed as chunk cache

The inverted index is different from the memory chunk.

This is described later chapter, Query Process.


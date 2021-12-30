# Filter the matched chunks

### Overview

As mentioned in the previous section,  the queriers retrieve matched chunk keys with a given query. &#x20;

However, they don't download the chunks from cache or BoltDB soon.

As a next step, they create an iterator to load chunks.

It is constructed with the chunks from all of the ingesters which are actual chunks and the chunk keys which are called "Lazy chunks".

![](<../.gitbook/assets/query-process-create-chunk-iterator.png>)

The lazy chunks don't have real ones at first but they will download them from cache or BoltDB once they are loaded in the iteration process.

The load process requests actual chunks for chunk cache at first, if no chunk is found in that, it asks AWS S3 for them.

If the querier downloads them from AWS S3, it will write them back to chunk cache.

After creating the iterator, the querier will start to load it to return a query result.

The elements in the iterator are loaded one by one and filter expressions in LogQL are evaluated at this time.

If the element is matched with the query, it will be added to the result.

The querier will repeat this until the response limit is reached and then return it to the query-frontend address specified in the query request.

![](<../.gitbook/assets/query-process-chunk-iterator.png>)

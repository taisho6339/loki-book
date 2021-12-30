# Query for all of ingesters

### Overview

After receiving a query, a querier sends query requests for all of the ingesters at first.

Ingesters have unflushed chunks in their memories and they are not in both cache and persistent storage, so the querier can retrieve the latest query results by doing that.

In addition, the ingesters have also inverted indexes for unflushed chunks so they select logs using them when receiving requests.

{% content-ref url="../ingestion-process/buffer-log-chunks.md" %}
[buffer-log-chunks.md](../ingestion-process/buffer-log-chunks.md)
{% endcontent-ref %}

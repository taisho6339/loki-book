# Query for all of ingesters

### Overview

After receiving a query, a querier sends query requests for all of the ingesters at first.

Ingesters have unflushed chunks in their memories and they are not in both cache and persistent storage.

Therefore, the queriers need to also ask them for ingesters.


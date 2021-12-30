# Failure stories

### Overview

Let me share my failure stories here.

### Querier can't select recent logs

In my environment, queriers sometimes couldn't select recent logs.

As I confirmed my configuration, `query_ingesters_within` was the root cause for that.

We need to set this parameter more than `max_chunk_age` otherwise, queriers won't query them for ingesters.

### Performance wouldn't improve even if using query splitting

I tried to improve my long time-range queries by using the query splitting feature.

However, they wouldn't be faster even if the feature was enabled.

The reason why it happened was because `max_query_parallelism` was default value, 32.

Querier didn't split a query more than the parameter.

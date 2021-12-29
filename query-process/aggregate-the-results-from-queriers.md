# Aggregate the results from queriers

The query-frontend waits for the results of the queries which were scheduled to queriers.

It aggregates, sorts them, and removes duplicates.

After that, it writes the result to query result cache and then returns it.

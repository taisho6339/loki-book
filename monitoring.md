# Monitoring

Average encoding chunk time

In this section, I'll share the PromQL I use for production monitoring.

```
sum(rate(loki_ingester_chunk_encode_time_seconds_sum{env="$environment"} [5m])) by(component,kubernetes_pod_name)
/ sum(rate(loki_ingester_chunk_encode_time_seconds_count{env="$environment"} [5m])) by(component,kubernetes_pod_name) * 1000
```



Average chunk utilization

```
sum(rate(loki_ingester_chunk_utilization_sum{env="$environment"} [5m])) by(component) / sum(rate(loki_ingester_chunk_utilization_count{env="$environment"} [5m])) by(component) * 100
```

Average chunk count per a query

```
sum(rate(loki_chunk_store_chunks_per_query_sum{env="$environment"} [5m]))by(component) /
 sum(rate(loki_chunk_store_chunks_per_query_count{env="$environment"} [5m]))by(component)
```

Chunk flush reason rate

```
sum(rate(loki_ingester_chunks_flushed_total{env="$environment"} [5m])) by(component, reason) > 0
```

Average chunk size

```
sum(rate(loki_ingester_chunk_size_bytes_sum{env="$environment", component="ingester"}[5m]))by(component) 
/ sum(rate(loki_ingester_chunk_size_bytes_count{env="$environment", component="ingester"}[5m]))by(component)
/ 1024^2
```

Average chunk compression rate

```
sum(rate(loki_ingester_chunk_compression_ratio_sum{env="$environment"}[5m]))by(component) 
/ sum(rate(loki_ingester_chunk_compression_ratio_count{env="$environment"}[5m]))by(component)
```

Average memory chunks

```
sum(loki_ingester_memory_chunks{env="$environment", component="ingester"})by(component, kubernetes_pod_name)
```

Average cache hit rate

```
sum(rate(loki_cache_hits{env="$environment", component="querier"} [5m]))by(component,exported_name) 
/ sum(rate(loki_cache_fetched_keys{env="$environment", component="querier"} [5m]))by(component,exported_name)
```

Average cache request duration

```
sum(rate(loki_cache_request_duration_seconds_sum[5m])) by(component,route, method, status_code) / sum(rate(loki_cache_request_duration_seconds_count[5m])) by(component,route, method, status_code) > 0
```

Average request duration

```
sum(rate(loki_request_duration_seconds_sum{verda_env="$environment"} [5m]))by(component,route, method, status_code) / sum(rate(loki_request_duration_seconds_count{env="$environment"} [5m]))by(component,route, method, status_code) > 0
```

Flush queue

```
sum(cortex_ingester_flush_queue_length{env="$environment"})by(verda_env, kubernetes_pod_name, compoent) > 0
```

Average query partition count by time interval splitting

```
sum(rate(loki_query_frontend_partitions_sum{env="$environment"} [5m]))by(verda_env, component)/ sum(rate(loki_query_frontend_partitions_count{env="$environment"} [5m]))by(env, component)
```

Average query partition count by sharding

```
sum(rate(loki_query_frontend_shard_factor_sum{verda_env="$environment"} [5m])) by(component, verda_env) / sum(rate(loki_query_frontend_shard_factor_count{env="$environment"} [5m])) by(component, env)
```


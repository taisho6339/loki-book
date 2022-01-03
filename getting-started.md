# What is Grafana Loki?

### What is Grafana Loki?

Loki has the following features.

* Ingest logs
* Search logs in Grafana Dashboard
* Create metrics based on logs
* Fire alert based on logs
* Multi-Tenancy

It can integrate with Prometheus, Grafana dashboard, and AlertManager seamlessly.

It doesn't have a storage engine itself and offloads it to other products like AWS S3, GCP CloudStorage, Cassandra, and so on.

It means that we can use scalable and low cost storage like S3 for storing.

It saves us a lot of money in case of a large number of logs and we don't need to work hard to keep storage high available and reliable.

### Use case

**People who want to store their large number of logs at low cost should use Loki!**

I need to store 20 TB logs per day at least, the cost should be very high so Loki is suitable.

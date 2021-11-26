# Getting Started

### What is Grafana Loki?

Loki enables us to use the following features.

* Ingest logs
* Search logs in Grafana Dashboard
* Create metrics based on logs
* Fire alert based on logs
* Multi-Tenancy

Loki can integrate with Prometheus, Grafana dashboard, and AlertManager seamlessly.

Loki doesn't have a storage engine itself and offloads it to other technologies like AWS S3, GCP CloudStorage, Cassandra, and so on.

It means that we can use hyper-scale and low-cost storage like S3 for storing and searching.

It saves us a lot of money in case of a large number of logs and we don't need to work hard to keep storage high available and reliable.

### Use case

Loki offloads storage engine to other services.

Therefore,** people who want to store their large number of logs with low cost and hyper-scale storage should use Loki!**

I need to store 20 TB logs per day at last, the cost should be very high so Loki is suitable.

### Deployment Mode

TBD

We can choose 3 kinds of deployment modes.

The official document can help you to know more detail.

{% embed url="https://grafana.com/docs/loki/next/fundamentals/architecture" %}

### How to install

TBD

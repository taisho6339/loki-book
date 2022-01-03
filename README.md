# Grafana Loki Deep Dive

### What is this book?

Grafana Loki is an awesome log monitoring platform provided as OSS.

However, it is hard to run in production because it is constructed with many microservices and we can't know how they work easily.

I aim to complement the official document and make the readers plan capacity, select parameters, and run Loki stably by themselves to provide this book.

This book shows you the following knowledge.

You can read only the part which you want to know.

* What's Loki?
* Dive into ingestion process
* Dive into query process
* Cache Strategy
* Design for failure
* Improve query performance
* Monitoring for Loki
* Some other tips

I hope that everyone enjoys "Loki Life"!

### Who should read this book?

I expect the readers as follows.

* Now considering introducing Loki
* Want more detailed information than the official document
* Be curious about distributed system

### Expected set up in this book

* Loki version: 2.4.1
* Chunk Storage: AWS S3
* Index Storage: BoltDB Shipper + AWS S3
* Deployment mode: Microservices

# Introduction

Grafana Loki is a great log monitoring platform provided as OSS.

However, it is hard to run in production because it is constructed with many microservices and we can't know how they work easily.

Therefore, I've been investigating for a long time to run in my production environment, which is now 2 TB / a day and aims to be 20 TB / a day.

I aim to complement the official document and make the readers plan capacity, select parameters, and run Loki stably by themselves

In this book, I'll provide detail about Loki and I hope that everyone enjoys "Loki Life"!

### Who should read this book?

I expect the readers as follows.

* Now considering introducing Loki
* Want more detailed information than the official document
* Be curious about distributed system

### Expected set up in this book

* Loki version: 2.4.1
* Chunk Storage: AWS S3
* Index Storage: BoltDB Shipper + AWS S3
* Cache: Memcache 1.6.12
* Deployment mode: Microservices




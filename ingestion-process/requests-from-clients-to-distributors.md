# 1. Requests from Clients to Distributors

![](<../.gitbook/assets/image (2).png>)

Loki clients construct log bodies and TenantID as HTTP requests.

Let's dive into the requests.

When it comes to TenantID, the clients annotate the ID to HTTP Header so that Loki can recognize which tenant of logs it receives.

About the request body, it has three kinds of fields, which are stream, timestamp, and actual log body.

Here is the actual format.

```
{
  "streams": [
    {
      "stream": {
        "label": "value"
      },
      "values": [
          [ "<unix epoch in nanoseconds>", "<log line>" ],
          [ "<unix epoch in nanoseconds>", "<log line>" ]
      ]
    }
  ]
}
```

#### What is Stream?

The logs should have some labels like Prometheus.

We call the unique pattern that is a combination of TenantID and label key-values "Stream".

The logs are going to be chunked and managed in every stream.

#### Validation

The requests from the clients are validated by Distributors.

What points does it validate?

Mainly, it checks the following.

* Label format
  * invalid format?
* Log Body
  * too long?
* Log timestamp
  * too future or too old?
* Rate limit

If the rate limit mode is 'global', distributors are clustered so that they can know the number of healthy instances in the cluster and calculate the ingestion rate for all over that.

It means that distributors are clustered for the validation.

If you want to know more detail about the clustering, please see here.&#x20;

// TODO:

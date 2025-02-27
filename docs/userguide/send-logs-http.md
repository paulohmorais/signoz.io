---
title: Sending Logs to SigNoz over HTTP
id: send-logs-http
---

## Overview

This guide provides detailed instructions on how to send logs to SigNoz using HTTP. Sending logs over HTTP offers flexibility, allowing users to create custom wrappers, directly transmit logs, or integrate existing loggers, making it a versatile choice for diverse use-cases.

## Payload Structure

The payload is an array of logs in JSON format. It follows a structure similar to [OTEL Logs Data Model](https://opentelemetry.io/docs/specs/otel/logs/data-model/).

Below is how the payload would look like:
```
[
  {
    "timestamp": <uint64>,
    "trace_id": <hex string>,
    "span_id": <hex string>,
    "trace_flags": <int>
    "severity_text": <string>,
    "severity_number": <int>,
    "attributes": <map>,
    "resources": <map>,
    "body": <string>,
  }
]
```
Here's a brief description of each field in the log record:

Field Name     |Description
---------------|--------------------------------------------
timestamp      |Time when the event occurred
trace_id        |Request trace id
span_id         |Request span id
trace_flags     |[W3C](https://www.w3.org/TR/trace-context/#trace-flags) trace flag
severity_text   |The severity text (also known as log level)
severity_number |Numerical value of the severity
attributes     |Additional information about the event
resources       |Describes the source of the log
body           |The body of the log record

To know more details about the different fields in a log record, you can check [this documentation](https://opentelemetry.io/docs/specs/otel/logs/data-model/#log-and-event-record-definition).

:::note

- `timestamp` is an uint64 showing time in **nanoseconds** since Unix epoch.
- You can use `message` instead of `body` to represent the body of a log record.

:::

<br></br>

## Additional Keys

Any additional keys present in the log record, apart from the ones mentioned in the above **payload structure** will be moved to the `attributes` map.

For example, if the JSON payload has fields like `host`, `method` etc. which are not a part of the standard payload structure,
  
  ```json
  [
    {
      "host": "myhost",
      "method": "GET",
      "body": "this is a log line"
    }
  ]
  ```

  Then they will be moved to `attributes` and the log record will be finally treated as: 

  ```json
  [
    {
      "attributes": {
        "host": "myhost",
        "method": "GET"
      },
      "body": "this is a log line"
    }
  ]
  ```


## Send logs to SigNoz Cloud

### Construct the cURL request 

You can use cURL to send your logs. Below is a sample cURL request which is used to send a JSON-formatted log record to a Signoz cloud ingestion endpoint for logging:

<!--- What other methods can we use apart from cURL ? --->

  ```bash
  curl --location 'https://ingest.<REGION>.signoz.cloud:443/logs/json/' \
  --header 'Content-Type: application/json' \
  --header 'signoz-access-token: <SIGNOZ_INGESTION_KEY>' \
  --data '[
      {
          "trace_id": "000000000000000018c51935df0b93b9",
          "span_id": "18c51935df0b93b9",
          "trace_flags": 0,
          "severity_text": "info",
          "severity_number": 4,
          "attributes": {
              "method": "GET",
              "path": "/api/users"
          },
          "resources": {
              "host": "myhost",
              "namespace": "prod"
          },
          "message": "This is a log line"
      }
  ]'
  ```

`<SIGNOZ_INGESTION_KEY>` is the API token provided by SigNoz. You can find your ingestion key from SigNoz cloud account details sent on your email.

`<REGION>` is the name of the region.
    
  Depending on the choice of your region for SigNoz Cloud, the OTLP endpoint will vary according to the table below:

  | Region | Endpoint                   |
  | ------ | -------------------------- |
  | US     | ingest.us.signoz.cloud:443 |
  | IN     | ingest.in.signoz.cloud:443 |
  | EU     | ingest.eu.signoz.cloud:443 |

:::note

To include a specific timestamp in your log, be sure to incorporate the `timestamp` field in your cURL request. If timestamp field is not mentioned, then it will take the timestamp when the log record was sent. For instance:

```bash
  curl --location 'https://ingest.<REGION>.signoz.cloud:443/logs/json/' \
  --header 'Content-Type: application/json' \
  --header 'signoz-access-token: <SIGNOZ_INGESTION_KEY>' \
  --data '[
      {
      "timestamp": 1698310066000000000, 
      "trace_id": "000000000000000018c51935df0b93b9", 
      ...
```

:::

### Verfiy your request

Once you run the above cURL request, you should be able to see it in SigNoz UI.

![JSON Data in log body](../../static/img/logs/http-log.webp)
  


## Send logs to Self-Hosted SigNoz

### Install SigNoz

Follow [this link](http://localhost:3000/docs/install/) for instructions on how to install self-hosted signoz.
Once you're done installing SigNoz, follow the steps below.

### Expose Port

Inside the `deploy/docker/clickhouse-setup` directory of your SigNoz self-hosted installation, you will find `docker-compose.yaml` file which you should modify to expose a port, in this case `8082`. Below is a code snippet of the modified file.


```yaml {8}
...
otel-collector:
    image: signoz/signoz-otel-collector:0.79.13
    command: ["--config=/etc/otel-collector-config.yaml"]
    volumes:
      - ./otel-collector-config.yaml:/etc/otel-collector-config.yaml
    ports:
      - "8082:8082"
...
```
### Add and include receiver

Inside the `deploy/docker/clickhouse-setup` directory of your SigNoz self-hosted installation, you will find `otel-collector-config.yaml` file which you should modify to add `httplogreceiver`.

```yaml {2-10}
receivers:
  httplogreceiver/json:
    endpoint: 0.0.0.0:8082
    source: json
...
```

Next modify the pipeline section inside `otel-collector-config.yaml` to include the `httplogreceiver` we have created above.

```yaml {4}
service:
    ....
    logs:
        receivers: [otlp, httplogreceiver/json]
        processors: [batch]
        exporters: [clickhouselogsexporter]
```

Now we can restart the **otel collector container** so that new changes are applied and we can send our logs to port `8082`.

### Construct the cURL request

You can use cURL to send your logs. Below is a sample cURL request which is used to send a JSON-formatted log record to SigNoz for logging:

```bash
curl --location 'http://<IP>:8082' \
--header 'Content-Type: application/json' \
--data '[
  {
      "trace_id": "000000000000000018c51935df0b93b9",
      "span_id": "18c51935df0b93b9",
      "trace_flags": 0,
      "severity_text": "info",
      "severity_number": 4,
      "attributes": {
          "method": "GET",
          "path": "/api/users"
      },
      "resources": {
          "host": "myhost",
          "namespace": "prod"
      },
      "message": "This is a log line"
  }
]'
```

 `<IP>` is the IP of the system where your collector is running.

  To know more details about `<IP>`, checkout this [troubleshooting](../install/troubleshooting.md#signoz-otel-collector-address-grid). 


:::note

To include a specific timestamp in your log, be sure to incorporate the `timestamp` field in your cURL request. If timestamp field is not mentioned, then it will take the timestamp when the log record was sent. For instance:

```bash
  curl --location 'https://ingest.<REGION>.signoz.cloud:443/logs/json/' \
  --header 'Content-Type: application/json' \
  --header 'signoz-access-token: <SIGNOZ_INGESTION_KEY>' \
  --data '[
      {
      "timestamp": 1698310066000000000, 
      "trace_id": "000000000000000018c51935df0b93b9", 
      ...
```

:::

### Verfiy your request

Once you run the above cURL request, you should be able to see it in SigNoz UI.

  ![test](../../static/img/logs/http-log.webp)
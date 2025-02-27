---
title: Collecting Application Logs from Log file
id: collect_logs_from_file
---
## Overview

This guide provides detailed instructions on configuring the OpenTelemetry Collector to read logs from a file and push them to SigNoz, enabling you to analyze your application logs effectively.

## Sample Log File
As an example, we can create a sample log file called `app.log` with the following dummy data: 
  ```
  This is log line 1
  This is log line 2
  This is log line 3
  ```
This file represents a log file of your application. You can choose any file which contains your application's log entries.

## Collect Logs in SigNoz Cloud

### Prerequisite

- SigNoz [cloud](https://signoz.io/teams/) account

Sending logs to SigNoz cloud can be achieved by following these simple steps:
- Installing OpenTelemetry Collector
- Configuring filelog receiver

### Install OpenTelemetry Collector 

The OpenTelemetry collector provides a vendor-neutral way to collect, process, and export your telemetry data such as logs, metrics, and traces.

You can install OpenTelemetry collector as an agent on your Virtual Machine by following this [guide](https://signoz.io/docs/tutorial/opentelemetry-binary-usage-in-virtual-machine/). 
  

### Configure filelog receiver

Modify the `config.yaml` file that you created while installing OTel collector in the previous step to include the filelog receiver. This involves specifying the path to your `app.log` file and setting the `start_at` parameter, which specifies where to start reading logs from the log file. For more fields that are available for filelog receiver please check [this link](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/filelogreceiver).

```yaml 
receivers:
  ...
  filelog/app:
    include: [ /tmp/app.log ] #include the full path to your log file
    start_at: end
...
```

:::note

The `start_at: end` configuration ensures that only newly added logs are transmitted. If you wish to include historical logs from the file, remember to modify `start_at` to `beginning`.

:::

<!--- 
For parsing logs of different formats you will have to use operators, you can read more about operators [here](https://signoz.io/docs/userguide/logs/#operators-for-parsing-and-manipulating-logs).
--->

### Update Pipelines Configuration

In the same `config.yaml` file, update the pipeline settings to include the new filelog receiver. This step is crucial for ensuring that the logs are correctly processed and sent to SigNoz.

```yaml {4}
service:
    ....
    logs:
        receivers: [otlp, filelog/app]
        processors: [batch]
        exporters: [otlp]
```

Now restart the OTel collector so that new changes are applied. The steps to run the OTel collector can be found [here](https://signoz.io/docs/tutorial/opentelemetry-binary-usage-in-virtual-machine/)

### Verify Export

The logs will be exported to SigNoz UI. If you add more entries to your `app.log` file they will also be visible in SigNoz UI.

<figure data-zoomable align='center'>
    <img src="/img/docs/application-logs-output.webp" alt="Logs of the dummy app.log file visible in SigNoz"/>
    <figcaption><i>Sample log file data shown in SigNoz Logs Explorer</i></figcaption>
</figure>

## Collecting Logs in self-hosted SigNoz

Collecting logs in Self-Hosted SigNoz can have two scenarios:
- SigNoz running on the same host
- SigNoz running on different host

### Running on the same host

If your self-hosted SigNoz is running on the same host, then you can follow these steps to collect your application logs.

#### Install SigNoz

You can install Self-Hosted SigNoz using the instructions [here](https://signoz.io/docs/install/docker/).


#### Modify Docker Compose file

In your self-hosted SigNoz setup, locate and edit the `docker-compose.yaml` file found in the `deploy/docker/clickhouse-setup` directory. You'll need to mount the log file of your application to the `tmp` directory of SigNoz OTel collector. 
  ```yaml {6}
    ...
    otel-collector:
    image: signoz/signoz-otel-collector:0.79.5
    command: ["--config=/etc/otel-collector-config.yaml"]
    volumes:
      - ~/<path>/app.log:/tmp/app.log
    ....
  ```

Replace `<path>` with the path where your log file is present. Please ensure that the file path is correctly specified.
  
#### Add filelog receiver

Add the filelog reciever to `otel-collector-config.yaml` which is present inside `deploy/docker/clickhouse-setup` directory in your self-hosted SigNoz setup. The configuratoin below tells the collector where to find your log file and how to start processing it.

  ```yaml {3-15}
  receivers:
    ...
    filelog:
      include: [ /tmp/app.log ]
      start_at: end
  ...
  ```

:::note

The `start_at: end` configuration ensures that only newly added logs are transmitted. If you wish to include historical logs from the file, remember to modify `start_at` to `beginning`.

:::

<!---
For parsing logs of different formats you will have to use operators, you can read more about operators [here](./logs.md#operators-for-parsing-and-manipulating-logs)
--->

For more fields that are available for filelog receiver please check [this link](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/filelogreceiver).

#### Update Pipeline configuration

Modify the pipeline inside `otel-collector-config.yaml` to include the filelog receiver. This step is crucial for ensuring that the logs are correctly processed and sent to SigNoz.

  ```yaml {4}
  service:
      ....
      logs:
          receivers: [otlp, filelog]
          processors: [batch]
          exporters: [clickhouselogsexporter]
  ```

Now, restart the OTel collector so that new changes are applied. You can find instructions to run OTel collector [here](https://signoz.io/docs/install/docker/)

#### Verify Export

The logs will be exported to SigNoz UI if there are no errors. If you add more entries to your `app.log` file they will also be visible in SigNoz.

<figure data-zoomable align='center'>
    <img src="/img/docs/application-logs-output.webp" alt="Logs of the dummy app.log file visible in SigNoz"/>
    <figcaption><i>Sample log file data shown in SigNoz Logs Explorer</i></figcaption>
</figure>



### Running on a different host

If you have a SigNoz running on a different host then you will have to run a OTel collector to export logs from your host to the host where SigNoz is running.

#### Create OTel collector configuration

You need to create an `otel-collector-config.yaml` file, this file defines how the OTel collector will process and forward logs to your SigNoz instance.

  ```yaml
  receivers:
    filelog:
      include: [ /tmp/app.log ]
      start_at: end
  processors:
    batch:
      send_batch_size: 10000
      send_batch_max_size: 11000
      timeout: 10s
  exporters:
    otlp/log:
      endpoint: http://<host>:<port>
      tls:
        insecure: true
  service:
    pipelines:
      logs:
        receivers: [filelog]
        processors: [batch]
        exporters: [ otlp/log ]
  ```
   
<!--- For parsing logs of different formats you will have to use operators, you can read more about operators [here](./logs.md#operators-for-parsing-and-manipulating-logs) --->
  

The parsed logs are batched up using the batch processor and then exported to the host where SigNoz is deployed. For finding the right host and port for your SigNoz cluster please follow the guide [here](../install/troubleshooting.md#signoz-otel-collector-address-grid).  

:::note

The `otlp/log` exporter in the above configuration file uses a `http` endpoint but if you want to use `https` you will have to provide the certificate and the key. You can read more about it [here](https://github.com/open-telemetry/opentelemetry-collector/blob/main/exporter/otlpexporter/README.md)

:::

#### Mount the log file

Run this docker command

```
docker run -d --name signoz-host-otel-collector --user root -v $(pwd)/app.log:/tmp/app.log:ro -v $(pwd)/otel-collector-config.yaml:/etc/otel/config.yaml signoz/signoz-otel-collector:0.79.0
```
The above command runs an OpenTelemetry collector provided by SigNoz in a Docker container. It runs in the background with root privileges, mounts a log file and a configuration file from the host to the container

After running the collector, if there are no errors your logs will be exported and will be visible in SigNoz.
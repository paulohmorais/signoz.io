---
id: linux-host-metrics
title: Linux Host Metrics
description: View linux host metrics SigNoz
hide_table_of_contents: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

### Overview

To export Linux metrics with OpenTelemetry, we need to install and configure the OpenTelemetry Collector Contrib to send metrics to SigNoz.

A full list of OpenTelemetry Collector Contrib supported hostmetrics can be found <a href = "https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/hostmetricsreceiver/README.md#getting-started" rel="noopener noreferrer nofollow" target="_blank" >here</a>

<Tabs>
  <TabItem value="cloud" label="SigNoz Cloud" default>


## Install otel-collector in your linux host

<Tabs>
  <TabItem value="DEB" label="DEB" default>

   ```sh
   sudo apt-get update
   sudo apt-get -y install wget systemctl
   wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.87.0/otelcol_0.87.0_linux_amd64.deb
   sudo dpkg -i otelcol_0.87.0_linux_amd64.deb
   ```

  </TabItem>
  <TabItem value="RPM" label="RPM" default>

   ```sh
   sudo yum update
   sudo yum -y install wget systemctl
   wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.87.0/otelcol_0.87.0_linux_amd64.rpm
   sudo rpm -ivh otelcol_0.87.0_linux_amd64.rpm
   ```

  </TabItem>
  <TabItem value="APK" label="APK" default>

   ```sh
   apk update
   apk add wget shadow
   wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.87.0/otelcol_0.87.0_linux_amd64.apk
   apk add --allow-untrusted otelcol_0.87.0_linux_amd64.apk
   ```

  </TabItem>
</Tabs>

   You can check the status of the installed service with:

   ```
   sudo systemctl status otelcol-contrib
   ```
   For check logs:
   ```bash
   sudo journalctl -u otelcol-contrib -f
   ```

   Now edit the config file `/etc/otelcol-contrib/config.yaml` add the below config:

   ```bash
   extensions:
     health_check:
     pprof:
       endpoint: 0.0.0.0:1777
     zpages:
       endpoint: 0.0.0.0:55679

   receivers:
     hostmetrics:
       collection_interval: 10s
       scrapers:
         cpu:
         disk:
         filesystem:
         load:
         memory:
         network:
         paging:

   processors:
     batch:
     resourcedetection:
       detectors: [env, system]
     cumulativetodelta:

   exporters:
     otlp:
       endpoint: <IP-or-Endpoint-of-SigNoz-OtelCollector>:4317
       tls:
         insecure: false
       headers:
         "signoz-access-token": "<SIGNOZ_INGESTION_KEY>"
   service:
     pipelines:
       metrics:
         receivers: [hostmetrics]
         processors: [cumulativetodelta, batch, resourcedetection]
         exporters: [otlp]

     extensions: [health_check, pprof, zpages]
  ```
  And restart OpenTelemetry Collector:

  ```bash
  sudo systemctl restart otelcol-contrib
  ```

</TabItem>

<TabItem value="self-host" label="Self-Host">

## Install otel-collector in your linux host

<Tabs>
  <TabItem value="DEB" label="DEB" default>

   ```sh
   sudo apt-get update
   sudo apt-get -y install wget systemctl
   wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.87.0/otelcol_0.87.0_linux_amd64.deb
   sudo dpkg -i otelcol_0.87.0_linux_amd64.deb
   ```

  </TabItem>
  <TabItem value="RPM" label="RPM" default>

   ```sh
   sudo yum update
   sudo yum -y install wget systemctl
   wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.87.0/otelcol_0.87.0_linux_amd64.rpm
   sudo rpm -ivh otelcol_0.87.0_linux_amd64.rpm
   ```

  </TabItem>
  <TabItem value="APK" label="APK" default>

   ```sh
   apk update
   apk add wget shadow
   wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.87.0/otelcol_0.87.0_linux_amd64.apk
   apk add --allow-untrusted otelcol_0.87.0_linux_amd64.apk
   ```

  </TabItem>
</Tabs>

   You can check the status of the installed service with:

   ```
   sudo systemctl status otelcol-contrib
   ```
   For check logs:
   ```bash
   sudo journalctl -u otelcol-contrib -f
   ```

   Now edit the config file `/etc/otelcol-contrib/config.yaml` add the below config:

   ```bash
   extensions:
     health_check:
     pprof:
       endpoint: 0.0.0.0:1777
     zpages:
       endpoint: 0.0.0.0:55679

   receivers:
     hostmetrics:
       collection_interval: 10s
       scrapers:
         cpu:
         disk:
         filesystem:
         load:
         memory:
         network:
         paging:

   processors:
     batch:
     resourcedetection:
       detectors: [env, system]
     cumulativetodelta:

   exporters:
     otlp:
       endpoint: <IP-or-Endpoint-of-SigNoz-OtelCollector>:4317
       tls:
         insecure: true
   service:
     pipelines:
       metrics:
         receivers: [hostmetrics]
         processors: [cumulativetodelta, batch, resourcedetection]
         exporters: [otlp]

     extensions: [health_check, pprof, zpages]
  ```
  And restart OpenTelemetry Collector:

  ```bash
  sudo systemctl restart otelcol-contrib
  ```

</TabItem>
</Tabs>

## Plot Metrics in SigNoz UI

**Import Dashboard**

   Import dashboard metrics of Linux Host
   from [here][1].

## Extra: Send metrics using Docker container

   You can send metrics using a Docker container, first we need to get the hostname of host:

   ```
   hostname
   ```

   Create a `config.yaml` file with config of this tutorial (SigNoz Cloud or Self-Host)

   Run a docker container passing the hostname of host:

   ```
   docker run -d \
    -v ./config.yaml:/etc/otelcol-contrib/config.yaml \
    -e OTEL_RESOURCE_ATTRIBUTES=host.name=HOSTNAME,os.type=linux \
    --name otel-collector \
    otel/opentelemetry-collector-contrib:latest
   ```

## Dashboard preview


<figure data-zoomable align='center'>
    <img src="/img/linux-host-metrics.png" alt="Linux host metrics Dashboard in SigNoz"/>
    <figcaption><i>Linux host metrics Dashboard in SigNoz</i></figcaption>
</figure>
<br></br>

---

[1]: https://github.com/SigNoz/dashboards/raw/main/hostmetrics/linux-host-metrics.json
---
id: create-a-custom-query
title: Create a Custom Query
sidebar_label: Create a Custom Query
---

import MetricsDefinition from '../shared/metrics-definition.md'
import AddPanelTimeSeries from '../shared/add-panel-time-series.md'
import SaveGraph from '../shared/save-graph.md'
import SeeAggregateFunctions from '../shared/see-aggregate-functions.md'


This page describes the elements that define a custom query and provides several examples of using the Query Builder to create custom queries.

## Elements that Define a Custom Query

To create a custom query, you must specify the following elements:

- Aggregation function
- Metric
- Where Clause
- Group By Clause
- Legend

The sections below describe these elements.

### Aggregation Function

Aggregation is a method of reducing the amount of data that SigNoz displays, and you can use it to identify patterns or outliers in your data. SigNoz allows you to perform both temporal and spatial aggregation.

### Temporal Aggregation

SigNoz re-aggregates into longer intervals the metrics you collect at a high frequency, allowing low-resolution time series to be pre-calculated or used in place of the original metric data. SigNoz auto-adjusts this interval based on the time range selected to limit the number of points it plots on the chart. The following example diagram shows the aggregation of data into ten seconds intervals:

![Temporal Aggregation](/img/docs/temporal_aggregation.webp)

### Spatial Aggregation

If you collect a metric and you don’t need all the attributes, you can remove unwanted attributes. This way, your metrics will only have the attributes that you need. The following diagram shows the aggregation of the entire set of time series into a single value at each interval of time:

![Spatial Aggregation](/img/docs/spatial_aggregation.webp)

### Supported Aggregation Functions

SigNoz supports the following aggregation functions:

- NOOP (No aggregation)
- COUNT (Number of values in each set of data)
- COUNT_DISTINCT (Number of distinct values in each set of data)
- SUM (Sum of all the values)
- AVG (Average of all the values)
- MAX (Maximum of all the values)
- MIN (Minimum of all the values)
- P05 (5th percentile of values)
- P10 (10th percentile of values)
- P20 (20th percentile of values)
- P25 (25th percentile of values)
- P50 (50th percentile of values)
- P75 (75th percentile of values)
- P90 (90th percentile of values)
- P95 (95th percentile of values)
- P99 (99th percentile of values)
- RATE (Rate of change of value for each time series)
- SUM_RATE (Sum of the rate of change of each time series)
- RATE_SUM (Rate of change on sum aggregated values)
- RATE_AVG (Rate of change on avg aggregated values)
- RATE_MAX (Rate of change on maa aggregated values)
- RATE_MIN (Rate of change on min aggregated values)


### How Metrics work in SigNoz?

<MetricsDefinition />

You can use a `where` clause to specify the filtering condition for your query, enabling you to select only data that meets the specified criteria. SigNoz will only plot data for which the where clause evaluates to `true`, and it’ll exclude everything else. For example, you can use a where clause to plot only the HTTP requests from a service named `foo`  that returned a `500` status code.

The following operators are supported:

- `IN`
- `NIN`
- `LIKE`
- `NLIKE`

Note that you can use the *`IN`* and *`NIN`* operators as an alternative for `=` and `!=` with a single value.

### Group By Clause

You can use the `Group By` clause to divide the results of your query based on the property you specify. The following example diagram shows how a `Group By` clause aggregates the same metric into separate groups, each group representing an AWS region:

![Group By Clause](/img/docs/aggregation_by_region.webp)

### Legend

Use the **Legend** text box to specify a legend name for your time series.


## Sample Examples to Create Custom Query

Writing PromQL or ClickHouse queries has a steep learning curve. The query builder is aimed at users who are not familiar with the Prometheus system or ClickHouse, and it allows you to write complex queries using an intuitive user interface.

This section walks you through the process of plotting graphs for different types of metrics. This will give you a good understanding of how to use the query builder.

### Prerequisites

- The sections below assume that your application is already instrumented. For details about how you can instrument your application, see the [Instrument Your Application](/docs/instrumentation/) section.
- The sections below assume that you are familiar with the basics of monitoring applications.

### Steps to Configure Host Metrics

Use host metrics to monitor the resource utilization of the hosts on which your application is running. SigNoz collects metrics describing the utilization of various system resources, such as CPU, disk, and network. This allows you to correlate both performance issues and errors observed in your application to unusual host metrics.

#### CPU Utilization

1. <AddPanelTimeSeries />

2. Enter the title of your new panel. The example screenshot creates a new panel named “CPU Utilization”:
  ![](/img/docs/new-panel-cpu-utilization.webp)

3. Choose `RATE_AVG` as the aggregate function:
  ![](/img/docs/choose-rate-avg.webp)

  <SeeAggregateFunctions />

4. From the **Metrics** drop-down, select `system_cpu_time`:
  ![](/img/docs/select-system-cpu-time.webp)

5. Specify that the state should not be `idle` by adding a filtering condition as shown below:
  ![](/img/docs/state-nin-idle.webp)

6. In the **Legend Format** text box, enter `{{state}}`:
  ![](/img/docs/legend-format-enter-state.webp)

7. Use the **Y Axis Unit** dropdown to specify that the value displayed on the Y-axis is a percentage and can take values from 0 to 100:
  ![](/img/docs/y-axis-percentage.webp)

8. To preview your query, select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
  ![](/img/docs/preview-cpu-utilization.webp)

9. _(Optional)_ To further drill down, you can plot the a separate graph for each state by specifying `state` in the **Group By** drop-down and selecting the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
  ![](/img/docs/group-by-specify-state.webp)

10. <SaveGraph />

#### Disk Saturation

Disk saturation means the disk is often accessed, and applications usually must wait before being able to read or write data. Follow the steps in this section to build a panel that shows the disk saturation metric.

1. <AddPanelTimeSeries />

2. Enter the title of your new panel. The example screenshot creates a new panel named “Disk Saturation”:
  ![](/img/docs/new-panel-disk-saturation.webp)

3. Choose `RATE` as the aggregate function:
  ![](/img/docs/choose-rate.webp)

    <SeeAggregateFunctions />

4. From the **Metrics** drop-down, select `system_weighted_io_time`:
  ![](/img/docs/select-system_weighted_io_time.webp)

5. In the **Legend Format** text box, enter `{{device}}`:
  ![](/img/docs/legend-format-enter-device.webp)

6. To preview your query, select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
  ![](/img/docs/preview-disk-saturation.webp)

7. (Optional) To further drill down, you can plot the a separate graph for each host name by specifying host_name in the **Group By** drop-down and selecting the **Stage & Run Query** button.

8. <SaveGraph />

#### Network Errors

1. <AddPanelTimeSeries />

2. Enter the title of your new panel. The example screenshot creates a new panel named “Network Errors”:

  ![](/img/docs/new-panel-network-errors.webp)

3. From the **Metrics** drop-down, select `system_network_errors`:

  ![](/img/docs/select-system_network_errors.webp)

4. _(Optional)_ Specify that you want to see the network errors for a specific host (this example uses `signoz-host`) by adding a filtering condition as shown below:

  ![](/img/docs/host-name-in-signoz.webp)

  Note that, for each network interface, SigNoz plots separate time series for sending and receiving data. The example screenshot below shows a system named `signoz-host` with two network interfaces (`lo` and `eth0`):

  ![](/img/docs/show-two-network-interfaces.webp)

5. In the **Legend Format** text box, enter `{{device}}-{{direction}}`:

  ![](/img/docs/legend-format-enter-device-direction.webp)

6. To preview your query, select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:

  ![](/img/docs/preview-network-errors.webp)

7. _(Optional)_ To further drill down, you can plot the a separate graph for sending and receiving data by specifying `direction` in the **Group By** drop-down and selecting the **Stage & Run Query** button.

8. <SaveGraph />

### Steps to Configure Application Metrics

Use application metrics to monitor the performance of your applications and identify any potential problems. Examples of applications metrics are percentile response time, error rates, request rates, memory and cpu usage.


#### Request Rate per Service

The example in this section calculates the request rate per service for the SigNoz application, but the steps you’ll learn will help you calculate this metric for your own application.

1. <AddPanelTimeSeries />

2. Enter the title of your new panel. The example screenshot creates a new panel named “Requests per Service”:

  ![](/img/docs/new-panel-requests-per-service.webp)

3. Choose `SUM_RATE` as the aggregate function:

  ![](/img/docs/choose-sum-rate.webp)

    <SeeAggregateFunctions />

4. Use the **Metrics** drop-down to specify that you want to plot the total number requests made to your application. The example screenshot below plots the total number of requests made to SigNoz:

  ![](/img/docs/signoz-total-calls.webp)

5. Specify the the services that you want to plot.

  To plot a single service, add a filtering condition as shown below:
  ![](/img/docs/plot-a-single-service.webp)

  To plot all the services, choose `service_name` from the **GROUP_BY** drop-down:
  ![](/img/docs/plot-all-services.webp)

6. _(Optional)_ If you are plotting all the services, enter `{{service_name}}` in the **Legend Format** text box.

7. To preview your query, select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
  ![](/img/docs/preview-requests-per-service.webp)

8. <SaveGraph />

#### Average Latency per Service

This example plots the average latency per service using a formula based on two queries. 

1. <AddPanelTimeSeries />

2. Enter the title of your new panel. The example screenshot creates a new panel named “Latency per Service”:

  ![](/img/docs/new-panel-latency-per-service.webp)

3. **Specify the First Query (A)**

  3.1 Under **A**, choose `SUM_RATE` as the aggregate function:
    ![](/img/docs/choose-sum-rate-avg-latency.webp)
    <SeeAggregateFunctions />
  
  3.2 Use the **Metrics** drop-down to specify that you want to plot the latency of your application’s requests. The example screenshot below plots the total latency for SigNoz:
    ![](/img/docs/total-latency-for-signoz.webp)

  3.3 Indicate that you want to plot the a separate graph each service, by specifying `service_name` in the **Group By** drop-down:
    ![](/img/docs/specify-service-name.webp)

  3.4 To preview your query, select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
    ![](/img/docs/preview-avg-latency-per-svc.webp)

4. **Specify the Second Query (B)**

  4.1 To add a new query, select the  **+Query** button.

  4.2 Under **B**, choose `SUM_RATE` as the aggregate function:

  4.3 To preview your query, select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
    ![](/img/docs/choose-sum-rate-under-b.webp)
    <SeeAggregateFunctions />

  4.4 Indicate that you want to plot the a separate graph each service, by specifying `service_name` in the **Group By** drop-down:
    ![](/img/docs/latency-per-svc-specify-svc-name.webp)

5. Hide **A** and **B** by selecting the corresponding eye icons:
    
  ![](/img/docs/avg-latency-per-svc-hide-a-b.webp)

6. Create a function that calculates `A/B` by selecting the **+Formula** button and entering `A/B` in the text box:
  ![](/img/docs/avg-latency-per-svc-create-formula.webp)

7. Select the **Stage & Run Query** button to preview your graph.

8. <SaveGraph />

#### Error Rate per Service

This example plots the error rate per service using a formula based on two queries.

1. <AddPanelTimeSeries />

2. Enter the title of your new panel. The example screenshot creates a new panel named “Error rates per service”:

  ![](/img/docs/error-rate-per-service.webp)


3. **Specify the First Query (A)**

  3.1 Choose `SUM_RATE` as the aggregate function:
    ![](/img/docs/error-rate-choose-sum-rate.webp)
    <SeeAggregateFunctions />
  
  3.2 Use the **Metrics** drop-down to specify that you want to plot the total number calls for your application. The example screenshot below plots the total number of calls for SigNoz:
    ![](/img/docs/plot-total-number-of-calls.webp)

  3.3 Specify that the you want to plot only the calls that failed by adding a filtering condition as shown below:
    ![](/img/docs/plot-the-calls-that-failed.webp)

  3.4 To plot a particular service add a new `WHERE clause`. The following example plots a service named `redis`:
    ![](/img/docs/plot-redis.webp)

4. **Specify the Second Query (B)**

  4.1 To add a new query, select the **+Query** button.

  4.2 Choose `SUM_RATE` as the aggregate function:
    ![](/img/docs/error-rate-choose-sum-rate-b.webp)

  4.3 Use the **Metrics** drop-down to specify that you want to plot the total number calls for your application. The example screenshot below plots the total number of calls for SigNoz:
    ![](/img/docs/plot-the-total-number-of-calls.webp)

  4.4 Indicate that the you do not want to plot the calls for the service you’ve previously specified (`redis`):
    ![](/img/docs/dont-plot-redis.webp)

5. Hide **A** and **B** by selecting the corresponding eye icons:
  ![](/img/docs/error-rate-hide-a-b.webp)

6. Create a function that calculates `A/B` by selecting the **+Formula** button and entering `A/B` in the text box:
  ![](/img/docs/error-rate-create-function.webp)

7. Select the **Stage & Run Query** button. SigNoz will plot a graph similar to the following one:
  ![](/img/docs/error-rate-preview.webp)

8. <SaveGraph />


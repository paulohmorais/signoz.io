---
title: Logs API 
id: overview
---

import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

## Overview

The SigNoz Logs API is a robust interface which enables developers to manage and analyze log data efficiently. This API facilitates various operations:

- **Searching Logs:** Allows users to search through log data based on specific criteria. For example, finding all logs where the error code is 500.

- **Paginating Logs:** Helps in navigating through large sets of log data in a manageable way. For instance, retrieving logs in batches of 100 for easy viewing.

- **Aggregating Logs:** Enables summarizing log data to extract meaningful insights, such as aggregating logs to count the number of errors per day.


## API Endpoint

Endpoint for Logs API: 

`POST` `https://{URL}/api/v3/query_range`

Replace `{URL}` with your instance URL, e.g., example.signoz.io.

## Prerequisites
Personal Access Token (PAT): To access this API, you need a PAT token. Obtain your PAT by contacting cloud-support@signoz.io.

## Authentication

Using the PAT Token: Add the PAT token to your request header as follows: 

```bash
SIGNOZ-API-KEY:{YOUR_PAT_TOKEN}
```

:::tip
Secure storage and handling of your PAT token is crucial to prevent unauthorized access.
:::

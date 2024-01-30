---
title: Trace API 
id: overview
---

import Tabs from "@theme/Tabs";
import TabItem from "@theme/TabItem";

## Overview

The SigNoz Trace API is a robust interface which enables developers to manage and analyze trace data efficiently. This API facilitates various operations:

- **Searching Traces:** Allows users to search for traces based on specific filters. For example, searching for traces based on the error code.

- **Aggregating Traces:** Allows users to aggregate traces based on specific filters. For example, aggregating traces based on the error code.

## API Endpoint

Endpoint for Trace API: 

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

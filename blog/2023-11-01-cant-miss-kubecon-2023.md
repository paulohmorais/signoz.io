---
title: "Can't-miss Kubecon 2023 Sessions for Observability"
slug: cant-miss-kubecon-2023
date: 2023-11-01
tags: [OpenTelemetry, Events]
authors: nicamellifera
description: Kubecon 2023 is coming up in just a week in Chicago. For engnineers concerned with observability, there are a number of talks you can't miss. I wrote up this guide to the talks I'm most looking forward to.
image: /img/blog/2023/11/kubecon-23-cover.jpg
hide_table_of_contents: false
keywords:
  - opentelemetry
  - kubecon
  - observability
  - ebpf
---

<head>
  <link rel="canonical" href="https://signoz.io/blog/cant-miss-kubecon-2023/"/>
</head>
Kubecon North America 2023 is coming up in just a week in Chicago. For engnineers concerned with observability, there are a number of talks you can't miss. I wrote up this guide to the talks I'm most looking forward to.

<!--truncate-->
![Cover Image](/img/blog/2023/11/kubecon-23-cover.webp)

## The talks I can't wait to see at Kubecon 2023

### 1. Migrating to OpenTelemetry

- **Speaker:** Jason Anderson & Kevin Broadbridge, Cruise
- **Event Link**: <a href = "https://kccncna2023.sched.com/event/1R2uc" rel="noopener noreferrer nofollow" target="_blank">November 9 • 2:00pm</a>
- **The Pitch**: Gain strategies for introducing and rolling out OpenTelemetry in your organization, including transitioning from other observability solutions.
- **Why I'll be There**: The transition to OpenTelemetry is the story for IT over the next few years. Most operations still used closed-source SaaS obervability solutions. OpenTelemetry is the future of observability, and I'll be there to hear the questions the audience asks, and see how Jason and Kevin recommend others tackle the problem

---

### 2. Building a "Debugging Paved Road" for Kubernetes

- **Speaker: Anusha Ragunathan & Kevin Downey, Intuit**
- **Event Link**: <a href = "https://sched.co/1R2nC" rel="noopener noreferrer nofollow" target="_blank">November 7 • 2:30pm</a>
- **The Pitch**: Learn how Intuit built a debugging framework for Kubernetes, leveraging Argo Workflows and Ephemeral Containers.
- **Why I'll be There**: Anusha gave an absolutely fantastic talk about the observability solutions at <a href = "https://www.youtube.com/watch?v=e5TZE9e2KPo" rel="noopener noreferrer nofollow" target="_blank">Intuit for Open Source Summit North America</a>. It really showed how a highly evolved operations team can give fundamental insights to production problems. This talk isn't on the same topic, but anyone who can build a dashboard as useful as the one I saw has useful insights on observability.

---

### 3. Business Observability & FinOps at Grafana Labs

- **Speaker**: Richard Hartmann, Grafana Labs
- **Event Link**: <a href = "https://colocatedeventsna2023.sched.com/event/1Rj1l/business-observability-finops-at-grafana-labs-richard-hartmann-grafana-labs" rel="noopener noreferrer nofollow" target="_blank">November 6 • 9:50am</a>
- **The Pitch**: Gain insights into cost-effective Kubernetes deployments using OpenCost and Prometheus. Learn actionable advice for replicating Grafana Labs' success in FinOps.
- **Why I'll be There**: I'm a big fan of Grafana Labs, and I'm excited to see how financial operations info can be observed and encoded with OpenTelemetry.

---

### 4. OTTL Me Why: Transforming Telemetry in the OpenTelemetry Collector

- **Speakers**: Tyler Helmuth, Honeycomb; Evan Bradley, Dynatrace
- **Event Link**: <a href = "https://colocatedeventsna2023.sched.com/event/1Rj2Y/ottl-me-why-transforming-telemetry-in-the-opentelemetry-collector-just-got-better-tyler-helmuth-honeycomb-evan-bradley-dynatrace" rel="noopener noreferrer nofollow" target="_blank">November 6 • 11:05am</a>
- **The Pitch**: Understand the OpenTelemetry Transformation Language (OTTL) for telemetry data transformation. Learn best practices and common mistakes.
- **Why I'll be There**: OTTL is the best tool for filtering, decorating, and modifying your OpenTelemetry data without making changes to your instrumentation. Many of us will only have access to configuring the OpenTelemetry Collector, so learning OTTL makes it easy to getting your data in shape.

---

### 5. How Much Overhead? How to Evaluate Observability Agent Performance

- **Speaker**: Braydon Kains, Google Cloud
- **Event Link**: <a href = "https://colocatedeventsna2023.sched.com/event/1Rj5U/how-much-overhead-how-to-evaluate-observability-agent-performance-braydon-kains-google-cloud" rel="noopener noreferrer nofollow" target="_blank">November 6 • 2:40pm</a>
- **The Pitch**: Learn to evaluate the performance and overhead of observability agents like OpenTelemetry Collector and Fluent Bit.
- **Why I'll be There**: How much increased resource use does it take to instrument your software? How much overhead does it take to send your data to a backend? How much overhead does it take to collect and analyze your data? I, like so many of us, tended to think of overhead as a fairly simple measurement, generally the increase in execution time once an application is instrumented. But the real measurement of overhead is a lot more detailed. The talk description discusses the real overhead of ingestion, processing, and exporting data. I answer questions on agent and instrumentation overhead, so I'm excited to get more information to help others.

---

### 6. Observability Considerations for Infrastructure Cost Optimization

- **Speaker**: Alolita Sharma, Apple
- **Event Link**: <a href = "https://colocatedeventsna2023.sched.com/event/1Rj78/observability-considerations-for-infrastructure-cost-optimization-alolita-sharma-apple" rel="noopener noreferrer nofollow" target="_blank">November 6 • 3:50pm</a>
- **The Pitch**: Explore open-source projects like OpenTelemetry and KubeCost for cloud-native infrastructure cost optimization.
- **Why I'll be There**: I'm excited to see how we can make concrete improvements in infrastructure costs with observability tools.

---

### 7. Collecting Low-Level Metrics with eBPF

- **Speaker**: Mauricio Vasquez Bernal, Microsoft
- **Event Link**: <a href = "https://kccncna2023.sched.com/event/1R2pH/collecting-low-level-metrics-with-ebpf-mauricio-vasquez-bernal-microsoft" rel="noopener noreferrer nofollow" target="_blank">November 7 • 5:25pm</a>
- **The Pitch**: Understand the fundamentals of metrics and eBPF for deep visibility into the operating system.
- **Why I'll be There**: I keep getting conflicting reccomendations around eBPF for observability. Tools tha do application monitoring with no instrumentation are impressive, but often have very high overhead. Some Go developers have mentioned on Reddit that they won't be using eBPF for observability due to security concerns, and Mauricio's talk shows how useful it can be. I'm interested to update my knowledge on the possible benefits of eBPF

---

### 8. eBPF + Wasm: Lightweight Observability on Steroids

- **Speakers**: **Michael Yuan, WasmEdge & Yusheng Zheng, eunomia-bpf**
- **Event Link**: <a href = "https://kccncna2023.sched.com/event/1R2uf" rel="noopener noreferrer nofollow" target="_blank">November 9 • 2:00pm</a>
- **The Pitch**: Learn about lightweight WasmEdge sandboxes for deploying and controlling eBPF applications in Kubernetes clusters.
- **Why I'll be There**: WasmEdge isn't something I've had a chance to try out, and Wasm has real potential for how it unlocks real plugin functionality. As with talk #7 on this list I also want another view on the role of eBPF in observability.

---

## I'd love to meet you at Kubecon!

If you're going to attend Kubecon in Chicago next week, I'd love to meet you and talk about OpenTelemetry, Observability, and how you can understand your systems in production. I'll buy the coffee if you'll share your stories about making a black box system more understandable. Find me on the <a href = "https://signoz.io/slack" rel="noopener noreferrer nofollow" target="_blank">SigNoz Slack</a>, on my <a href = "https://twitter.com/Serverless_Mom" rel="noopener noreferrer nofollow" target="_blank">Twitter</a> (DM's are open!), and on my <a href = "https://www.linkedin.com/in/otel-mom" rel="noopener noreferrer nofollow" target="_blank">LinkedIn</a>.
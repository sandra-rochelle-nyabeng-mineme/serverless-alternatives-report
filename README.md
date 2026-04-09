# ☁️ Serverless Service Alternatives Report
### Azure · AWS · GCP — A Comparative Analysis

> **Course:** Cloud Development and Operations  
> **Assignment:** Serverless Service Alternatives Report  
> **Name:** Sandra rochelle nyabeng mineme 

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Quick Reference: Service Equivalents at a Glance](#quick-reference)
3. [Azure Functions vs AWS Lambda vs GCP Cloud Functions](#1-azure-functions-vs-aws-lambda-vs-gcp-cloud-functions)
4. [Durable Functions vs AWS Step Functions vs GCP Workflows](#2-durable-functions-vs-aws-step-functions-vs-gcp-workflows)
5. [Azure Logic Apps vs AWS Step Functions (Express) / EventBridge Pipes vs GCP Workflows + Eventarc](#3-azure-logic-apps-vs-aws-eventbridge-pipes-vs-gcp-workflows--eventarc)
6. [Azure Service Bus vs AWS SQS/SNS vs GCP Pub/Sub](#4-azure-service-bus-vs-aws-sqssns-vs-gcp-pubsub)
7. [Azure Event Grid vs AWS EventBridge vs GCP Eventarc](#5-azure-event-grid-vs-aws-eventbridge-vs-gcp-eventarc)
8. [Azure Event Hubs vs AWS Kinesis vs GCP Pub/Sub (Streaming)](#6-azure-event-hubs-vs-aws-kinesis-vs-gcp-pubsub-streaming)
9. [Overall Strengths & Weaknesses Summary](#overall-summary)
10. [Conclusion](#conclusion)

---

## Introduction

This report examines six core Azure serverless services studied in the course and maps each to its closest equivalent in Amazon Web Services (AWS) and Google Cloud Platform (GCP). For each service trio, the report provides an overview, feature comparison, integration options, monitoring capabilities, pricing model, and a narrative analysis of strengths and weaknesses from a serverless architecture perspective.

The goal is to equip cloud practitioners with the knowledge to make informed decisions when designing or migrating serverless workloads across cloud providers.

---

## Quick Reference

| Azure Service | AWS Equivalent | GCP Equivalent |
|---|---|---|
| Azure Functions | AWS Lambda | Cloud Functions / Cloud Run Functions |
| Durable Functions | AWS Step Functions | GCP Workflows |
| Azure Logic Apps | AWS EventBridge Pipes / Step Functions Express | GCP Workflows + Eventarc |
| Azure Service Bus | AWS SQS + SNS | GCP Pub/Sub |
| Azure Event Grid | AWS EventBridge | GCP Eventarc |
| Azure Event Hubs | AWS Kinesis Data Streams | GCP Pub/Sub (Streaming mode) |

---

## 1. Azure Functions vs AWS Lambda vs GCP Cloud Functions

### Overview

| | Azure Functions | AWS Lambda | GCP Cloud Functions |
|---|---|---|---|
| **Provider** | Microsoft Azure | Amazon Web Services | Google Cloud Platform |
| **Description** | Event-driven, serverless compute platform supporting multiple languages and hosting plans | Industry-leading FaaS (Function as a Service) platform with broad runtime support | Lightweight FaaS offering; Gen 2 is built on Cloud Run for better scalability |
| **GA Since** | 2016 | 2014 | 2016 |

### Core Features

| Feature | Azure Functions | AWS Lambda | GCP Cloud Functions |
|---|---|---|---|
| **Supported Runtimes** | C#, Java, JavaScript, Python, PowerShell, Go, TypeScript | Node.js, Python, Ruby, Java, Go, .NET, custom runtime | Node.js, Python, Go, Java, .NET, Ruby, PHP |
| **Max Execution Time** | 230 sec (Consumption); unlimited (Premium/Dedicated) | 15 minutes | 60 min (Gen 2) |
| **Max Memory** | 1.5 GB (Consumption); 14 GB (Premium) | 10 GB | 32 GB (Gen 2) |
| **Cold Start** | Yes (mitigated with Premium plan) | Yes (mitigated with Provisioned Concurrency) | Yes (mitigated with min-instances) |
| **Triggers** | HTTP, Timer, Blob, Queue, Event Grid, Event Hubs, Service Bus, Cosmos DB, SignalR | HTTP, API Gateway, S3, DynamoDB, SQS, SNS, Kinesis, EventBridge, CloudWatch | HTTP, Pub/Sub, Cloud Storage, Firestore, Firebase, Cloud Scheduler |
| **Bindings** | Rich input/output bindings (Blob, Queue, Table, Cosmos DB, etc.) | No native bindings; use SDK | No native bindings; use SDK |
| **Deployment Units** | Function App (groups multiple functions) | Function (individual), Lambda Layer (shared code) | Individual function or Cloud Run service |
| **VNet Integration** | Yes (Premium/Dedicated) | Yes (VPC) | Yes (VPC connector) |

### Integration Options

| | Azure Functions | AWS Lambda | GCP Cloud Functions |
|---|---|---|---|
| **CI/CD** | GitHub Actions, Azure DevOps, Azure Pipelines | GitHub Actions, AWS CodePipeline, CodeBuild | GitHub Actions, Cloud Build, Cloud Deploy |
| **API Gateway** | Azure API Management | Amazon API Gateway | Cloud Endpoints / API Gateway |
| **Secrets Management** | Azure Key Vault | AWS Secrets Manager / Parameter Store | Secret Manager |
| **IAM** | Azure RBAC / Managed Identity | IAM Roles / Policies | Workload Identity / Service Accounts |
| **Container Support** | Custom handlers, Azure Container Apps | Lambda Container Images (up to 10 GB) | Cloud Run (Gen 2) |

### Monitoring & Observability

| | Azure Functions | AWS Lambda | GCP Cloud Functions |
|---|---|---|---|
| **Native Monitoring** | Azure Monitor, Application Insights | Amazon CloudWatch, X-Ray | Cloud Monitoring, Cloud Logging, Cloud Trace |
| **Logging** | App Insights (built-in), Log Analytics | CloudWatch Logs | Cloud Logging |
| **Distributed Tracing** | Application Insights (end-to-end) | AWS X-Ray | Cloud Trace |
| **Alerting** | Azure Alerts | CloudWatch Alarms | Cloud Alerting Policies |
| **Dashboard** | Azure Portal, Grafana | CloudWatch Dashboards | Cloud Monitoring Dashboards |

### Pricing Model

| | Azure Functions | AWS Lambda | GCP Cloud Functions |
|---|---|---|---|
| **Free Tier** | 1M requests + 400,000 GB-s/month | 1M requests + 400,000 GB-s/month | 2M invocations/month |
| **Per Request** | $0.20 per 1M | $0.20 per 1M | $0.40 per 1M |
| **Per GB-s** | $0.000016 | $0.0000166667 | $0.0000025/GHz-s (CPU-based) |
| **Premium/Reserved** | Premium plan (always-warm) | Provisioned Concurrency (per instance-hour) | Min instances (per instance-hour) |

### Analysis

Azure Functions stands out for its **binding system** — the ability to declaratively wire inputs and outputs to Azure services (Blob Storage, Cosmos DB, Service Bus, etc.) without writing SDK boilerplate is a significant developer productivity advantage. This is unique to Azure and has no direct equivalent in Lambda or Cloud Functions.

**AWS Lambda** is the most mature of the three, with the broadest trigger ecosystem and the largest community. Its 15-minute maximum execution time and 10 GB memory ceiling make it suitable for heavier workloads. Lambda Layers allow code reuse across functions. The trade-off is that Lambda requires more configuration to achieve what Azure does declaratively via bindings.

**GCP Cloud Functions Gen 2** (built on Cloud Run) has closed the gap significantly, offering longer timeouts and higher memory. Its pricing model is CPU-based rather than memory-based, which can be more cost-effective for CPU-intensive workloads. However, its trigger ecosystem is smaller — primarily Pub/Sub and HTTP — making it less versatile out of the box.

**Key takeaway:** For teams already in the Azure ecosystem, Azure Functions provides the most integrated experience. For multi-cloud or greenfield projects, Lambda's maturity and ecosystem breadth make it the default choice.

---

## 2. Durable Functions vs AWS Step Functions vs GCP Workflows

### Overview

| | Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| **Provider** | Microsoft Azure | Amazon Web Services | Google Cloud Platform |
| **Description** | Extension of Azure Functions enabling stateful workflows in serverless code using the virtual actor model | Managed workflow orchestration using JSON/YAML state machines (Amazon States Language) | Fully managed workflow orchestration service using YAML-based syntax |
| **Programming Model** | Code-first (C#, JavaScript, Python, Java, PowerShell) | Configuration-first (ASL: Amazon States Language) | YAML-based syntax with expressions |

### Core Features

| Feature | Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| **Orchestration Patterns** | Chaining, Fan-out/Fan-in, Human interaction, Aggregator, Saga | Sequential, Parallel, Map, Choice, Wait | Sequential, Parallel, Iteration, Conditional |
| **State Management** | Automatic (Azure Storage / Netherite backend) | Managed by Step Functions service | Managed by Workflows service |
| **Chaining** | `yield context.call_activity()` in code | `Task` state → `Next` transitions | `call` steps with `next` |
| **Fan-out / Fan-in** | `yield context.task_all([...])` | `Parallel` or `Map` state | `parallel` branch block |
| **Human Interaction / Wait** | `wait_for_external_event()` | `.waitForTaskToken` callback pattern | `await` with callback endpoint |
| **Versioning** | Supported via `get_status()` / instance management | Task Token + versioning via state machine versions | Workflow revisions |
| **Max Workflow Duration** | Unlimited (event-sourced) | 1 year (Standard); 5 min (Express) | 1 year |
| **Execution History** | Azure Storage Tables / Event Sourcing | Execution history in console/API | Execution logs in Cloud Logging |

### Integration Options

| | Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| **Invoke External Services** | Via activity functions (HTTP, SDK calls) | `Task` states with SDK integrations (200+ AWS services) | `http.post`, `http.get` steps; Google API connectors |
| **Event-driven Start** | HTTP, Event Grid, Queue triggers | API Gateway, EventBridge, SQS, S3 | Cloud Scheduler, Pub/Sub, Eventarc |
| **Error Handling** | Try/catch in orchestrator code | `Catch` / `Retry` in state definitions | `try/except` YAML blocks |
| **CI/CD** | Azure Pipelines, GitHub Actions | CodePipeline + CloudFormation / CDK | Cloud Build + Terraform / Deployment Manager |

### Monitoring & Observability

| | Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| **Visual Designer** | No built-in visual designer | AWS Step Functions Visual Editor | Workflow graph in Cloud Console |
| **Execution Tracking** | Durable Functions Monitor (OSS tool), Application Insights | Step Functions console ; full execution history | Cloud Console execution log |
| **Logging** | App Insights, Azure Monitor | CloudWatch Logs | Cloud Logging |
| **Tracing** | Application Insights (distributed) | X-Ray integration | Cloud Trace |

### Pricing Model

| | Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| **Billing Unit** | Function executions + storage (Azure Tables/Blobs) | State transitions | Workflow steps |
| **Standard/Orchestration** | Function consumption pricing + storage | $0.025 per 1,000 state transitions | $0.01 per 1,000 internal steps |
| **Express/Fast** | N/A | $1.00 per 1M state transitions (Express) | $0.01 per 1,000 steps |
| **Free Tier** | Included in Azure Functions free tier | 4,000 state transitions/month | 5,000 internal steps/month |

### Analysis

**Durable Functions** takes a code-first approach that many developers find natural; orchestrations are written as regular async code with `yield`/`await`, hiding the complexity of checkpointing and replay. This is ideal for teams comfortable with programming languages who prefer not to learn a new DSL. The trade-off is limited visual tooling and the need to understand the replay execution model carefully to avoid side effects in orchestrators.

**AWS Step Functions** is configuration-first using Amazon States Language (JSON/YAML). This makes workflows inspectable, version-controllable, and auditable without reading code. The visual designer is a major advantage for non-developer stakeholders. Step Functions Standard has a 1-year limit and full history; Express Workflows are optimized for high-volume, short-duration flows at a lower cost. The 200+ direct SDK integrations (calling DynamoDB, SQS, ECS, etc. directly from state definitions without Lambda) is a significant differentiation.

**GCP Workflows** is newer and more limited in native integrations but is YAML-based and clean. Its main strength is tight integration with Google APIs and the ability to call any HTTP endpoint natively. For GCP-native workloads, it covers most use cases. However, it lacks the visual richness of Step Functions and the code-first convenience of Durable Functions.

**Key takeaway:** Step Functions is the most enterprise-ready of the three with its visual editor and deep AWS integrations. Durable Functions wins on developer experience for code-centric teams. GCP Workflows is a solid choice for GCP-native projects but lags in ecosystem depth.

---

## 3. Azure Logic Apps vs AWS EventBridge Pipes vs GCP Workflows + Eventarc

### Overview

| | Azure Logic Apps | AWS EventBridge Pipes | GCP Workflows + Eventarc |
|---|---|---|---|
| **Provider** | Microsoft Azure | Amazon Web Services | Google Cloud Platform |
| **Description** | Low-code/no-code integration platform with 400+ connectors for automating workflows across apps and services | Point-to-point event pipe connecting sources to targets with optional filtering and enrichment | Combination of Workflows (orchestration) and Eventarc (event routing) to replicate Logic Apps capabilities |
| **Target User** | Business analysts, citizen developers, integration engineers | Developers building event-driven integrations | Developers familiar with GCP tooling |

### Core Features

| Feature | Azure Logic Apps | AWS EventBridge Pipes | GCP Workflows + Eventarc |
|---|---|---|---|
| **No-Code Designer** | Yes — drag-and-drop visual designer | No (API/CDK/console configuration) | No |
| **Connectors / Integrations** | 400+ (SalesForce, SAP, Office 365, Dynamics, etc.) | ~20 source/target pairs natively | Via http.get/http.post to any API |
| **Triggers** | HTTP, Schedule (CRON), Event Grid, Service Bus, Blob, Teams, Outlook, SAP, Salesforce, etc. | SQS, DynamoDB Streams, Kinesis, Kafka, MQ → targets like Lambda, SNS, SQS, EventBridge | Eventarc sources: Pub/Sub, Cloud Storage, Firestore, 60+ Google event types |
| **Stateful vs Stateless** | Both (Consumption = stateful; Standard = stateful + stateless) | Stateless (pipe execution) | Stateful via Workflows; Stateless via Eventarc alone |
| **B2B / EDI Support** | Yes (Integration Account — AS2, X12, EDIFACT) | No | No |
| **Conditional Logic** | Yes (Condition, Switch, Until, For Each) | Basic filter/enrichment only | Full conditional support via Workflows YAML |
| **Error Handling** | Scope + configure run after (success/failure/skip/timeout) | Retry policy on pipe | try/except in Workflows |

### Integration Options

| | Azure Logic Apps | AWS EventBridge Pipes | GCP Workflows + Eventarc |
|---|---|---|---|
| **Enterprise Apps** | SAP, Salesforce, Dynamics 365, ServiceNow, Oracle (native connectors) | Via Lambda enrichment + custom integrations | Via HTTP calls; no native enterprise connectors |
| **Microsoft 365** | Native (Teams, Outlook, SharePoint, OneDrive) | Via Lambda + Graph API | Via HTTP + OAuth |
| **CI/CD** | ARM/Bicep templates, Azure DevOps, GitHub Actions | CloudFormation, CDK, SAM | Cloud Build, Terraform |
| **Custom Code** | Inline JavaScript, Azure Functions call-out | Lambda enrichment function | Workflows expression language + Cloud Functions |

### Monitoring & Observability

| | Azure Logic Apps | AWS EventBridge Pipes | GCP Workflows + Eventarc |
|---|---|---|---|
| **Run History** | Built-in run history with input/output per step | CloudWatch Logs + Pipe metrics | Cloud Logging + Workflows execution log |
| **Visual Trace** | Yes — each step shows green/red with payload | No | Workflows execution graph |
| **Alerts** | Azure Monitor Alerts | CloudWatch Alarms | Cloud Monitoring Alerts |
| **Audit** | Azure Activity Log | CloudTrail | Cloud Audit Logs |

### Pricing Model

| | Azure Logic Apps | AWS EventBridge Pipes | GCP Workflows + Eventarc |
|---|---|---|---|
| **Model** | Per action/trigger execution (Consumption); flat monthly (Standard) | Per event processed through pipe | Workflows steps + Eventarc event deliveries |
| **Free Tier** | None (Consumption billed per action) | 1M events/month free | 5,000 steps/month + 100K events/month |
| **Standard Plan** | From ~$193/month (single-tenant) | N/A | N/A |
| **Consumption** | ~$0.000025 per action execution | ~$0.40 per 1M events | ~$0.01 per 1,000 steps |

### Analysis

**Azure Logic Apps** is the most complete iPaaS (Integration Platform as a Service) offering among the three. Its 400+ connectors and no-code visual designer make it accessible to non-developers, and its B2B/EDI support (AS2, X12, EDIFACT) is unmatched. For enterprise integration scenarios, especially those involving Microsoft products, Logic Apps has no peer. The consumption pricing model can become expensive at scale since every connector action is billed.

**AWS EventBridge Pipes** is a narrower service focused on point-to-point event routing with filtering and enrichment. It doesn't aspire to be a full Logic Apps replacement; it's better understood as a complement to Step Functions and Lambda for event-driven architectures. It lacks a visual designer and has fewer native connectors, but it integrates tightly into the AWS ecosystem.

**GCP's combination** of Workflows + Eventarc approximates Logic Apps functionality for GCP-native services but requires developer effort to construct what Logic Apps delivers out of the box with clicks. There are no enterprise SaaS connectors and no visual canvas. It's best suited for GCP-internal automation rather than cross-system enterprise integration.

**Key takeaway:** Azure Logic Apps is the clear leader for enterprise integration and business process automation. For developer-centric, AWS-native event routing, EventBridge Pipes + Step Functions covers most needs. GCP's answer is functional but code-heavy, making it better suited for technical teams than citizen developers.

---

## 4. Azure Service Bus vs AWS SQS/SNS vs GCP Pub/Sub

### Overview

| | Azure Service Bus | AWS SQS + SNS | GCP Pub/Sub |
|---|---|---|---|
| **Provider** | Microsoft Azure | Amazon Web Services | Google Cloud Platform |
| **Description** | Enterprise-grade message broker with queues and topics/subscriptions; supports advanced messaging patterns | SQS = managed message queuing; SNS = managed pub/sub fan-out notifications. Together they replicate Service Bus capabilities | Fully managed, scalable messaging service combining queue and pub/sub patterns |
| **Messaging Model** | Queues (point-to-point) + Topics/Subscriptions (pub/sub) | SQS (queues) + SNS (topics/fan-out) | Topics + Subscriptions (unified model) |

### Core Features

| Feature | Azure Service Bus | AWS SQS | AWS SNS | GCP Pub/Sub |
|---|---|---|---|---|
| **Message Ordering** | Yes (sessions) | Yes (FIFO queues) | No (fan-out only) | Ordering keys (per-key ordering) |
| **Dead Letter Queue** | Yes (built-in DLQ) | Yes (redrive policy) | Yes (SQS DLQ for subscriptions) | Yes (dead letter topic) |
| **Message TTL** | Up to 14 days | Up to 14 days | N/A | 7 days (default), up to 31 days |
| **Max Message Size** | 256 KB (Standard); 100 MB (Premium) | 256 KB (up to 2 GB via S3 extended) | 256 KB | 10 MB |
| **Duplicate Detection** | Yes | Yes (FIFO queues — 5 min window) | No | At-least-once delivery; deduplication via message ID |
| **Transactions** | Yes (atomic send/receive across entities) | No | No | No |
| **Message Deferral** | Yes | No | No | No |
| **Scheduled Messages** | Yes | Message delay (up to 15 min for Standard) | No | Yes (message ordering + timestamp) |
| **Topic Subscriptions** | Yes (with SQL filters) | N/A | Yes (fan-out to SQS, Lambda, HTTP, email, SMS) | Yes (push + pull; filter policies) |
| **Sessions (Consumer Groups)** | Yes (session-based receivers) | FIFO queues with group IDs | N/A | Subscription-level consumer groups |

### Integration Options

| | Azure Service Bus | AWS SQS/SNS | GCP Pub/Sub |
|---|---|---|---|
| **Functions / Compute** | Azure Functions (trigger + binding), Logic Apps | Lambda (event source mapping), Step Functions | Cloud Functions, Cloud Run, Dataflow |
| **Message Routing** | Topic filters (SQL-like expressions) | SNS filter policies (attribute-based) | Pub/Sub filter expressions |
| **Monitoring** | Azure Monitor, Service Bus Explorer | CloudWatch, SQS/SNS metrics | Cloud Monitoring, Pub/Sub metrics |
| **Event Grid Bridge** | Yes (Azure Event Grid → Service Bus) | Yes (EventBridge → SQS/SNS) | Yes (Eventarc → Pub/Sub) |
| **Protocol Support** | AMQP 1.0, HTTPS, SBMP | HTTPS, SQS Wire Protocol | HTTPS, gRPC |

### Monitoring & Observability

| | Azure Service Bus | AWS SQS/SNS | GCP Pub/Sub |
|---|---|---|---|
| **Key Metrics** | Active messages, DLQ count, server errors, incoming/outgoing messages | Number of messages sent/received, approximate age of oldest message | Subscription backlog, oldest unacked message age, publish rate |
| **Logging** | Diagnostic Logs → Log Analytics | CloudWatch Logs (via Lambda) | Cloud Logging |
| **Alerting** | Azure Monitor Alerts on queue depth, DLQ | CloudWatch Alarms | Cloud Monitoring Alert Policies |
| **Tooling** | Service Bus Explorer (GUI), Azure Portal | AWS Console, SQS/SNS CLI | Cloud Console, gcloud CLI |

### Pricing Model

| | Azure Service Bus | AWS SQS | AWS SNS | GCP Pub/Sub |
|---|---|---|---|---|
| **Free Tier** | None | 1M requests/month | 1M publishes/month; 100K HTTP deliveries | 10 GB/month |
| **Standard** | $0.10/million operations | $0.40/million requests | $0.50/million publishes | $0.04/GB |
| **Premium** | Fixed hourly (messaging units) | FIFO: $0.50/million | N/A | N/A |
| **Data Egress** | Included in operations cost | Standard network egress | Standard network egress | Standard network egress |

### Analysis

**Azure Service Bus** is the most feature-rich message broker of the three. Its support for transactions, message deferral, scheduled delivery, sessions, and rich SQL-based topic filters makes it the best fit for complex enterprise messaging patterns. The premium tier adds dedicated resources and VNet integration. It is particularly strong for scenarios requiring exactly-once processing semantics and ordered delivery with correlation.

**AWS SQS + SNS** together replicate Service Bus's combined queue + pub/sub capability, but they are separate services requiring integration. SQS Standard offers nearly unlimited throughput while FIFO queues provide ordering and deduplication guarantees. SNS handles fan-out well. The limitation is that you lose transactional capabilities and some of the richer filtering that Service Bus provides natively. However, SQS's simplicity and tight Lambda integration make it very popular in event-driven AWS architectures.

**GCP Pub/Sub** is elegant in its unified model; a single service handles both queueing and pub/sub through subscriptions. Its throughput is exceptionally high and it shines in streaming/analytics pipelines (tightly integrated with Dataflow, BigQuery). The 10 MB message size limit is a constraint for large payload scenarios. It lacks advanced features like transactions, deferral, and SQL-based filtering.

**Key takeaway:** Service Bus is the enterprise messaging champion. SQS/SNS wins for AWS-native simplicity and Lambda integration. GCP Pub/Sub is the best choice for high-throughput streaming and analytics pipelines.

---

## 5. Azure Event Grid vs AWS EventBridge vs GCP Eventarc

### Overview

| | Azure Event Grid | AWS EventBridge | GCP Eventarc |
|---|---|---|---|
| **Provider** | Microsoft Azure | Amazon Web Services | Google Cloud Platform |
| **Description** | Fully managed event routing service for reactive, event-driven architectures; routes events from Azure resources to handlers | Serverless event bus with advanced filtering, schema registry, and SaaS event ingestion | Managed event routing service that connects GCP services, custom apps, and third-party sources using Pub/Sub as the backbone |
| **Event Model** | Push-based (HTTP webhook / Azure service handlers) | Push-based (targets: Lambda, SQS, Kinesis, Step Functions, API GW, etc.) | Push-based (targets: Cloud Run, Cloud Functions, GKE, Workflows) |

### Core Features

| Feature | Azure Event Grid | AWS EventBridge | GCP Eventarc |
|---|---|---|---|
| **Event Sources** | Azure resources (Blob, Resource Groups, Subscriptions), custom topics, Event Hubs, IoT Hub, Service Bus | AWS services (100+), SaaS partners (Salesforce, Zendesk, etc.), custom event buses | GCP services (60+), custom sources via Pub/Sub, Audit Log events |
| **Event Filtering** | Subject prefix/suffix filter, event type filter | Content-based filtering on any JSON field (up to 5 filters) | Attribute-based filtering |
| **Event Schema** | Cloud Events 1.0, Event Grid Schema | AWS Events, Cloud Events 1.0 | Cloud Events 1.0 |
| **Schema Registry** | Yes (Event Grid Schema Registry) | Yes (EventBridge Schema Registry — auto-discovers) | No built-in schema registry |
| **Dead Letter** | Yes (Blob Storage DLQ) | Yes (SQS DLQ) | Via Pub/Sub DLQ |
| **Retry Policy** | Up to 24 hours, exponential backoff | Up to 24 hours, configurable | Via Pub/Sub retry |
| **Event Delivery Guarantee** | At-least-once | At-least-once | At-least-once |
| **Custom Events** | Yes (custom topics) | Yes (custom event buses) | Yes (via custom channels + Pub/Sub) |
| **SaaS Partners** | Microsoft partners (GitHub, Auth0, etc.) | 35+ SaaS partners (Salesforce, Zendesk, PagerDuty, etc.) | Limited (primarily GCP ecosystem) |

### Integration Options

| | Azure Event Grid | AWS EventBridge | GCP Eventarc |
|---|---|---|---|
| **Compute Targets** | Azure Functions, Logic Apps, Event Hubs, Service Bus, Webhooks, Azure Automation | Lambda, Step Functions, SQS, SNS, Kinesis, API Gateway, CloudWatch | Cloud Run, Cloud Functions, GKE, Workflows |
| **Cross-Account / Cross-Region** | Cross-subscription (same tenant) | Cross-account, cross-region event buses | Cross-project via Pub/Sub |
| **Event Replay** | Yes (Event Grid namespace — preview) | Yes (EventBridge Archive & Replay) | Limited (via Pub/Sub message retention) |
| **API Destinations** | Yes (any HTTP endpoint with auth) | Yes (API destinations with OAuth / API Key) | Yes (Cloud Run HTTP targets) |

### Monitoring & Observability

| | Azure Event Grid | AWS EventBridge | GCP Eventarc |
|---|---|---|---|
| **Metrics** | Published events, matched events, delivery failures, delivery latency | Matched rule invocations, failed invocations, throttled rules | Event counts, delivery failures (via Cloud Monitoring) |
| **Logging** | Diagnostic Logs → Log Analytics Workspace | CloudTrail, CloudWatch Logs | Cloud Audit Logs, Cloud Logging |
| **Alerting** | Azure Monitor Alerts | CloudWatch Alarms | Cloud Monitoring Alerts |
| **Dead Letter Tracking** | Blob Storage inspection | SQS DLQ inspection | Pub/Sub DLQ inspection |

### Pricing Model

| | Azure Event Grid | AWS EventBridge | GCP Eventarc |
|---|---|---|---|
| **Free Tier** | 100,000 operations/month | 1M events/month (custom bus free) | 100K events/month |
| **Per Million Events** | $0.60 (after free tier) | $1.00 (custom) / $0.00 (AWS service events) | $0.10/million |
| **Schema Registry** | Included | $0.10/schema/month | N/A |
| **Archive & Replay** | Preview pricing | $0.023/GB archived | Via Pub/Sub storage pricing |

### Analysis

**Azure Event Grid** is tightly integrated with the Azure resource model; every resource change in Azure can automatically emit an event without any configuration. This makes it the natural reactive backbone for Azure-native architectures. Its advanced filtering (subject, event type, prefix/suffix matching) and support for CloudEvents 1.0 make it interoperable. Event Grid Namespaces (in preview) add MQTT support for IoT scenarios.

**AWS EventBridge** is the most powerful of the three, particularly thanks to its Schema Registry with auto-discovery (it can detect event schemas by observing traffic), its 35+ SaaS partner integrations (Salesforce, Zendesk, Shopify), and its Archive & Replay capability. The ability to replay historical events for debugging or reprocessing is a significant operational advantage. EventBridge Pipes (separate service) extends this with point-to-point enrichment pipelines.

**GCP Eventarc** uses Pub/Sub as its backbone, which gives it reliability and scalability but also adds complexity. It covers GCP-native events well (60+ services) but has fewer SaaS partners and no schema registry. It's simpler and less expensive than the other two for GCP-native use cases but is not as enterprise-ready for complex event routing needs.

**Key takeaway:** EventBridge is the enterprise event routing leader with SaaS integrations and replay. Event Grid is the best fit for Azure-native reactive architectures. Eventarc is the pragmatic GCP-native choice for internal event-driven workflows.

---

## 6. Azure Event Hubs vs AWS Kinesis vs GCP Pub/Sub (Streaming)

### Overview

| | Azure Event Hubs | AWS Kinesis Data Streams | GCP Pub/Sub (Streaming) |
|---|---|---|---|
| **Provider** | Microsoft Azure | Amazon Web Services | Google Cloud Platform |
| **Description** | Big data streaming platform and event ingestion service capable of processing millions of events per second; Kafka-compatible | Managed real-time data streaming service with shard-based partitioning for ordered, replayable streams | The same Pub/Sub service used for messaging, but capable of high-throughput streaming when combined with Dataflow |
| **Primary Use Case** | Telemetry, log streaming, clickstream, IoT data pipelines | Real-time analytics, log aggregation, IoT, click-stream | Application messaging, log streaming, data pipeline ingestion |

### Core Features

| Feature | Azure Event Hubs | AWS Kinesis Data Streams | GCP Pub/Sub |
|---|---|---|---|
| **Throughput Unit** | Throughput Units (TUs) or Processing Units (Premium) | Shards (1 MB/s write, 2 MB/s read per shard) | Auto-scaling (no manual partitioning) |
| **Partition Model** | Named partitions (configurable, up to 32 Standard; 2000 Premium) | Shards (explicitly provisioned or on-demand) | Internal partitioning (transparent to user) |
| **Max Retention** | 1 day (Standard); 90 days (Premium/Dedicated) | 1 day (default); 7 days (extended); 365 days (long-term) | 7 days (default); up to 31 days |
| **Replay / Seek** | Yes (offset-based consumer groups) | Yes (shard iterator — TRIM_HORIZON, AT_TIMESTAMP, etc.) | Limited (snapshot / seek to time or snapshot) |
| **Kafka Compatibility** | Yes (Event Hubs for Kafka — surface compatible) | No (AWS MSK for Kafka) | No |
| **Consumer Groups** | Yes | Yes (per-shard iterator per consumer) | Yes (subscriptions act as consumer groups) |
| **Capture to Storage** | Yes (Event Hubs Capture → Blob / ADLS) | Yes (Kinesis Firehose → S3, Redshift, OpenSearch) | Yes (Pub/Sub → Cloud Storage via subscription) |
| **Max Event Size** | 1 MB | 1 MB | 10 MB |
| **Protocol** | AMQP, HTTPS, Apache Kafka | HTTPS, Kinesis Agent | HTTPS, gRPC |

### Integration Options

| | Azure Event Hubs | AWS Kinesis Data Streams | GCP Pub/Sub |
|---|---|---|---|
| **Stream Processing** | Azure Stream Analytics, Spark (HDInsight/Databricks), Azure Functions | Kinesis Data Analytics (Apache Flink), Lambda, Spark (EMR) | Cloud Dataflow (Apache Beam), BigQuery subscriptions, Spark (Dataproc) |
| **Analytics Sink** | Azure Synapse, Power BI, ADX (Data Explorer) | Amazon Redshift, OpenSearch, S3 + Athena | BigQuery (direct subscription), Looker |
| **IoT Integration** | Azure IoT Hub → Event Hubs routing | AWS IoT Core → Kinesis | Cloud IoT Core → Pub/Sub |
| **Kafka Migration** | Native (Kafka surface on Event Hubs) | AWS MSK (separate service) | Kafka connector (open-source) |
| **CI/CD** | Azure Pipelines, ARM/Bicep | CloudFormation, CDK | Cloud Build, Terraform |

### Monitoring & Observability

| | Azure Event Hubs | AWS Kinesis Data Streams | GCP Pub/Sub |
|---|---|---|---|
| **Key Metrics** | Incoming requests, throttled requests, consumer lag (via SDK), incoming/outgoing bytes | GetRecords.IteratorAgeMilliseconds (consumer lag), PutRecord success, ReadProvisionedThroughputExceeded | Subscription backlog bytes, oldest unacked message age, publish rate |
| **Native Dashboards** | Azure Monitor, Event Hubs namespace metrics | CloudWatch (per-shard metrics) | Cloud Monitoring |
| **Consumer Lag Tracking** | Via checkpoint offsets in consumer group; Azure Monitor | IteratorAgeMilliseconds in CloudWatch | Oldest unacked message age |
| **Logging** | Diagnostic Logs → Log Analytics | CloudWatch Logs | Cloud Logging |

### Pricing Model

| | Azure Event Hubs | AWS Kinesis Data Streams | GCP Pub/Sub |
|---|---|---|---|
| **Billing Unit** | Throughput Units (hourly) + ingress/egress GB | Shard-hours + PUT payload units | Data volume (GB) published + delivered |
| **Free Tier** | None | None | 10 GB/month |
| **Base Cost** | ~$0.015/TU/hour (Standard) | ~$0.015/shard/hour | $0.04/GB (publisher) + $0.04/GB (subscriber) |
| **Capture / Firehose** | $0.028/hour per namespace (Capture) | Kinesis Firehose: $0.029/GB ingested | Cloud Storage egress pricing |
| **Kafka Surface** | Included in Event Hubs price | AWS MSK separate pricing | N/A |
| **Scaling Model** | Manual TU scaling (or auto-inflate) | Manual shard management or on-demand mode | Fully automatic |

### Analysis

**Azure Event Hubs** differentiates itself with **Kafka protocol compatibility** — organizations can point existing Kafka clients at Event Hubs without code changes, making migration dramatically simpler. Its Capture feature automatically archives streams to Azure Blob Storage or Data Lake, enabling a lambda architecture pattern without additional services. At the Premium/Dedicated tier, it supports up to 2000 partitions for very high parallelism.

**AWS Kinesis Data Streams** is mature and reliable but requires manual shard provisioning, which can be operationally burdensome. The on-demand mode (announced in 2021) alleviates this but at a higher per-unit cost. Kinesis's ecosystem is rich: Kinesis Data Firehose handles delivery to S3/Redshift/OpenSearch, and Kinesis Data Analytics (Apache Flink) enables in-stream processing. The 365-day retention option (long-term retention) is unique — no other managed streaming service offers this out of the box.

**GCP Pub/Sub** in streaming mode is the simplest to operate — no partition management, no shard math. It scales automatically and integrates natively with BigQuery (via direct BigQuery subscriptions, data lands in BigQuery without a Dataflow pipeline) and Cloud Dataflow for Apache Beam pipelines. The trade-off is limited replay semantics (no offset-based replay like Kafka/Kinesis; only time-based seek or snapshot), which makes it less suitable for audit-trail or exactly-once reprocessing scenarios.

**Key takeaway:** Event Hubs is the best choice for Kafka-compatible workloads or Microsoft analytics stacks. Kinesis excels in AWS-native pipelines with long retention needs. GCP Pub/Sub is the simplest operationally and best integrated with BigQuery and Dataflow for analytics.

---

## Overall Summary

### Strengths & Weaknesses by Provider

| Dimension | Azure | AWS | GCP |
|---|---|---|---|
| **Compute (FaaS)** | Azure Functions : rich trigger/binding system; cold starts on Consumption plan | AWS Lambda : most mature FaaS; widest native trigger set across AWS services | Cloud Functions Gen 2 (built on Cloud Run); fewer native triggers than Lambda |
| **Orchestration** | Durable Functions : code-first, stateful workflows in C#/Python/JS | AWS Step Functions : visual workflow designer; 200+ SDK integrations | Google Workflows : YAML-based, clean syntax; fewer third-party integrations |
| **Low-code Integration** | Logic Apps : 400+ connectors including B2B/EDI; no-code designer | EventBridge Pipes : limited point-to-point; no full iPaaS equivalent | No native iPaaS offering; integrations require custom code or third-party tools |
| **Messaging (Queues)** | Service Bus : supports transactions, message deferral, and sessions | SQS : mature, reliable; supports FIFO ordering but no native transactions | Pub/Sub — unified push/pull model; no native transaction support |
| **Event Routing** | Event Grid : native Azure resource event routing; custom topics | EventBridge : strong SaaS partner integrations; supports archive and replay | Eventarc : simple routing model; fewer native SaaS event sources |
| **Streaming / Big Data** | Event Hubs : Kafka-compatible protocol; Capture to Blob/ADLS | Kinesis : up to 365-day retention; Firehose for delivery pipelines | Pub/Sub Lite / Dataflow : auto-scaling, no-ops; native BigQuery integration |
| **Pricing Transparency** | Complex : billed by Throughput Units, executions, and Logic App actions separately | Complex : billed by shards (Kinesis) and state transitions (Step Functions) | Simpler : volume-based pricing; easier to estimate at scale |
| **Multi-cloud / Open Standards** | Supports Kafka protocol, CloudEvents, and AMQP | Proprietary-leaning; limited native open standard support | Strong open standards support ; CloudEvents, gRPC, Knative |
| **Enterprise / SaaS Connectors** | Logic Apps : deep connectors for SAP, Salesforce, Dynamics 365 | EventBridge : 35+ SaaS partners natively supported | Limited native SaaS connectors; relies on custom integrations |
| **Microsoft Ecosystem Fit** | Native : best fit for Microsoft/Azure-first organizations | Partial : accessible via Lambda and third-party connectors | Weak : limited native Microsoft service support |
| **Google Ecosystem Fit** | Partial : accessible via connectors | Partial : accessible via connectors | Native : best fit for BigQuery, Vertex AI, and GCP-first organizations |

---

## Conclusion

Each cloud provider has invested heavily in serverless and event-driven capabilities, and each has distinct strengths:

- **Azure** is the clear leader for organizations in the Microsoft ecosystem, enterprise integration (Logic Apps), and Kafka-compatible streaming (Event Hubs). Its binding system in Azure Functions remains a unique developer productivity feature with no direct equivalent elsewhere.

- **AWS** offers the most mature, battle-tested set of serverless services. Lambda's ecosystem, Step Functions' visual orchestration with 200+ SDK integrations, EventBridge's SaaS partner network, and Kinesis's long-term retention make AWS the default choice for greenfield enterprise projects.

- **GCP** excels in simplicity, open standards, and analytics-oriented architectures. Its automatic scaling (no shard/partition management in Pub/Sub), native BigQuery integration, and CloudEvents support make it compelling for data-heavy, analytics-first teams. It lags in enterprise connectors and iPaaS capabilities.

For a new serverless project, provider selection should be driven by existing ecosystem investments, team expertise, and the specific pattern mix (integration-heavy → Azure; AWS-native services → AWS; analytics/BigQuery → GCP). In multi-cloud architectures, standard protocols (Kafka, CloudEvents, AMQP, HTTPS) provide the interoperability layer to mix and match these services.

---

*All pricing reflects public list prices as of 2025 and is subject to change. Refer to each provider's official pricing page for current rates.*

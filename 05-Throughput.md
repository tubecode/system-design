# Throughput

---

## 1. Simple Definition
Throughput is **how much work gets done in a given amount of time**.

In short: **how many records, files, or events can your system process per second/minute/hour?**

---

## 2. Technical Definition (Data Engineering Context)
Throughput is the volume of data successfully processed, moved, or ingested by a pipeline within a unit of time.

It is commonly measured as:
- **Records per second** (e.g., 10,000 rows/sec)
- **MB or GB per second** (e.g., 500 MB/min)
- **Events per second** (e.g., Kafka consuming 1M events/sec)

Higher throughput = system handles more data in less time.  
Low throughput = bottleneck exists somewhere in the pipeline.

---

## 3. Real-World Analogy
Imagine a **toll booth on a highway**:

| Toll Booth Scenario | Data Engineering Equivalent |
|---|---|
| Cars arriving at the booth | Records/events arriving at the pipeline |
| Toll booth processes one car at a time | Single-threaded pipeline processes one record at a time |
| Adding more toll lanes | Parallelism / partitioning / scaling out |
| Cars passing through per hour | = **Throughput** |

One booth with 10 lanes processes far more cars per hour than one lane.  
That increase in cars per hour = **higher throughput**.  
A traffic jam at one lane = **bottleneck reducing throughput**.

---

## 4. Why This Concept Is Important in Data Engineering

| Area | Why Throughput Matters |
|---|---|
| **ETL Pipelines** | Low throughput = pipeline takes hours to process data that should take minutes. |
| **Streaming Systems** | If consumer throughput < producer throughput, messages pile up and lag increases. |
| **Data Loading** | Bulk loading into Snowflake/BigQuery requires high throughput to meet SLAs. |
| **Big Data Processing** | Spark jobs tuned for throughput handle terabytes efficiently. |
| **API Ingestion** | Low API throughput limits how fast data can be pulled from source systems. |

In your CDCM dashboard, low warehouse throughput = slow `COUNT(*)` queries = long load times.

---

## 5. Tools & Technologies Used

| Category | Tools | Purpose |
|---|---|---|
| **High-throughput ingestion** | Apache Kafka, AWS Kinesis, Azure Event Hubs | Handle millions of events per second |
| **Batch ingestion** | Apache Sqoop, AWS Glue, Fivetran | Bulk data movement with high throughput |
| **Processing** | Apache Spark, Flink, dbt | Parallel processing to maximize throughput |
| **Storage** | Snowflake, BigQuery, Redshift, Delta Lake | Optimized for high-throughput reads and writes |
| **Orchestration** | Airflow, Prefect, Dagster | Schedule and monitor pipeline throughput SLAs |
| **Monitoring** | Grafana, Datadog, Prometheus | Track throughput metrics and detect bottlenecks |

---

## 6. One-Line Interview Summary

> **"Throughput is the amount of data a system can process per unit of time — maximizing it through parallelism, partitioning, and efficient resource use is key to building scalable data pipelines."**

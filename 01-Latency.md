# Latency

---

## 1. Simple Definition
Latency is the **delay** between when something happens and when you see or receive the result.

In short: **how long does it take?**

---

## 2. Technical Definition (Data Engineering Context)
Latency is the time elapsed between a data event being generated (at the source) and that data being available for consumption (in a dashboard, table, or system).

It is measured in milliseconds, seconds, minutes, or hours depending on the pipeline type.

- **Low latency** = data arrives very quickly (near real-time).
- **High latency** = data takes longer to arrive (batch, scheduled jobs).

---

## 3. Real-World Analogy
Imagine ordering food at a restaurant:

| Restaurant Scenario | Data Engineering Equivalent |
|---|---|
| You place an order | Data is generated at source (app, sensor, database) |
| Kitchen prepares food | ETL/pipeline processes the data |
| Waiter brings food to your table | Data lands in dashboard/warehouse |
| Time from order to plate arriving | = **Latency** |

If the kitchen is slow, or there are 100 orders queued, you wait longer.  
That wait = **high latency**.  
A fast kitchen with pre-prepped ingredients = **low latency**.

---

## 4. Why This Concept Is Important in Data Engineering

| Area | Why Latency Matters |
|---|---|
| **ETL Pipelines** | Batch jobs running every 6 hours = 6-hour latency. Business decisions are based on old data. |
| **Real-Time Analytics** | Fraud detection, stock trading, live dashboards need sub-second latency. |
| **Data Warehouses** | Query latency impacts how fast dashboards load. |
| **Streaming Pipelines** | Kafka, Spark Streaming designed specifically to minimize latency. |
| **APIs / Microservices** | High latency in upstream APIs delays your pipeline. |

---

## 5. Tools & Technologies Used

| Category | Tools | Purpose |
|---|---|---|
| **Low-latency ingestion** | Apache Kafka, AWS Kinesis, Azure Event Hubs | Stream data in real-time |
| **Stream processing** | Apache Flink, Spark Streaming, Kafka Streams | Process data with millisecond latency |
| **Batch processing** | Apache Spark, dbt, Airflow | Scheduled, higher latency acceptable |
| **Storage (fast reads)** | Snowflake, BigQuery, Redshift, Redis | Optimized for low query latency |
| **Caching** | Redis, Streamlit cache, Memcached | Reduce repeated query latency |
| **Monitoring** | Datadog, Grafana, Prometheus | Measure and alert on latency |
| **Orchestration** | Airflow, Prefect, dbt | Control pipeline scheduling and latency SLAs |

---

## 6. One-Line Interview Summary

> **"Latency is the time delay between data being created and being available for use — minimizing it is critical for real-time analytics, and managing it well separates good data pipelines from great ones."**

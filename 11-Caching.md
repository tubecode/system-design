# Caching in Data Engineering

## 1. Simple Definition
**Caching** is the practice of storing the result of an expensive operation in a fast, temporary location so the next time you need it, you get it instantly without repeating the work.

Think of it like keeping your most-used items on your desk instead of going to the storage room every time you need them.

---

## 2. Technical Definition (Data Engineering Context)
Caching is the technique of **storing frequently accessed or expensive-to-compute data in fast memory** (RAM) or an intermediate layer, so future requests are served faster without hitting the original slow source.

**In data engineering:**
- Cache a SQL query result so 100 users don't each re-run it against the database
- Cache a Spark DataFrame in memory to avoid re-reading from HDFS/S3 every transformation
- Cache API responses to avoid repeated network calls
- Cache aggregations so dashboards load in milliseconds instead of seconds

**Types of caching:**
| Type | Where | Speed | Use Case |
|------|-------|-------|----------|
| **In-memory cache** | RAM | Fastest | Frequently read small datasets |
| **Disk cache** | SSD | Fast | Larger datasets that don't fit in RAM |
| **Distributed cache** | Cluster RAM | Fast + scalable | Multi-node Spark jobs |
| **Query result cache** | Database layer | Fast | Repeated identical queries |
| **CDN cache** | Edge servers | Fast globally | API responses, dashboard data |

**Key terms:**
- **Cache hit** = data found in cache → instant response
- **Cache miss** = data NOT in cache → must fetch from source (slow)
- **TTL (Time To Live)** = how long cached data stays valid before being refreshed
- **Cache eviction** = removing old/unused data from cache to make room

---

## 3. Real-World Analogy
**Chef and Recipe Book:**

**Without Cache:**
- Customer orders pasta
- Chef walks to the storage room, finds recipe book, reads recipe, walks back, then cooks
- Next customer orders same pasta
- Chef walks to storage room again, finds recipe book again, reads again...
- Every order = same wasted walking time

**With Cache:**
- Chef writes the top 5 most-ordered recipes on a sticky note on the fridge (cache)
- Customer orders pasta
- Chef checks fridge sticky note → found immediately → starts cooking
- Next 100 customers order same pasta → chef uses sticky note every time (instant)
- TTL: At end of week, chef updates sticky note with new popular dishes (cache refresh)

**Maps to Data Engineering:**
- Storage room = database / data warehouse (slow to query)
- Recipe book = raw source data
- Sticky note = cache (stored in fast memory)
- Chef = application / pipeline fetching data
- Same pasta order = same query run repeatedly
- Updating sticky note = cache invalidation / refresh

**Dashboard Example:**
```
WITHOUT CACHE:
User 1 → Dashboard → Query Snowflake → 30 seconds → Result
User 2 → Dashboard → Query Snowflake → 30 seconds → Result  (same query again!)
User 3 → Dashboard → Query Snowflake → 30 seconds → Result  (and again!)

WITH CACHE (TTL = 1 hour):
User 1 → Dashboard → Query Snowflake → 30 seconds → Result (stored in cache)
User 2 → Dashboard → Cache Hit! → 0.01 seconds → Same Result
User 3 → Dashboard → Cache Hit! → 0.01 seconds → Same Result
```

---

## 4. Why This Concept Is Important in Data Engineering

| Scenario | Why Caching Matters |
|----------|---------------------|
| **BI dashboards** | 50 users open same dashboard → without cache, 50 identical heavy queries hit warehouse → slow + expensive |
| **Spark jobs** | Same DataFrame used in 5 transformations → without cache, Spark re-reads from S3 each time → 5x I/O cost |
| **ETL pipelines** | Lookup tables used repeatedly (e.g., currency conversion) → cache once, reuse throughout pipeline |
| **API rate limits** | External API allows 100 calls/day → cache responses to avoid hitting limit |
| **Real-time streaming** | Kafka consumer needs enrichment data (e.g., customer info) → cache in Redis to avoid DB lookup per event |
| **ML model serving** | Feature lookup for predictions → cache precomputed features for low-latency inference |
| **Cost reduction** | Cloud warehouses charge per query → caching results cuts query cost dramatically |

**Business impact:**
- Faster dashboards → better user experience
- Reduced database load → fewer crashes
- Lower cloud costs → less compute/query charges
- Higher throughput → more requests handled

---

## 5. Tools & Technologies Used

### **In-Memory / Distributed Cache**
- **Redis** — most popular cache, key-value store, supports TTL
- **Memcached** — fast, lightweight, for simple caching
- **Apache Ignite** — distributed in-memory computing
- **Hazelcast** — distributed cache for Java/JVM

### **Spark-Level Caching**
- **df.cache()** — cache Spark DataFrame in cluster memory
- **df.persist()** — cache with configurable storage level (memory, disk)
- **Broadcast variables** — cache small lookup tables across Spark workers

### **Database / Query Cache**
- **Snowflake Result Cache** — automatically caches query results for 24h (free!)
- **BigQuery BI Engine** — in-memory cache for repeated queries
- **Redshift Result Cache** — caches SELECT results
- **PostgreSQL** — shared_buffers for data page caching

### **Application-Level Caching**
- **Streamlit @st.cache_data** — cache Python function results in web apps
- **Python functools.lru_cache** — cache function results in Python
- **Django cache framework** — web application-level cache

### **CDN/Edge Caching**
- **Cloudflare** — cache API responses globally
- **AWS CloudFront** — cache content at edge locations

### **Monitoring Cache Performance**
- **Redis Insights** — monitor hit/miss ratio
- **Prometheus + Grafana** — track cache metrics
- **Snowflake Query History** — see if result cache was used

---

## 6. One-Line Interview Summary
**"Caching stores the results of expensive operations in a fast-access layer so repeated requests are served instantly, reducing database load, latency, and infrastructure costs."**

---

## Key Takeaway
**Caching = Speed + Cost Savings**

Always ask these three questions when considering caching:
1. **Is this data read frequently?** → Good cache candidate
2. **Is it expensive to compute/fetch?** → Even better candidate
3. **How often does it change?** → Determines your TTL

**Anti-pattern:** Caching data that changes every second → stale data misleads users

**Golden rule:** Cache reads, never cache writes. Cache what is slow, not what is fast.

---

## 7. Common Eviction Policies
When cache is full, old data must be removed to make room for new data. This is called **eviction**. The policy decides *which* data gets removed first.

| Policy | Full Name | How It Works | Best For |
|--------|-----------|--------------|----------|
| **LRU** | Least Recently Used | Removes data that hasn't been accessed for the longest time | General-purpose caching (most common) |
| **LFU** | Least Frequently Used | Removes data that has been accessed the fewest times overall | Popularity-based access patterns |
| **MRU** | Most Recently Used | Removes the most recently accessed data first | When older data is more likely to be reused |
| **FIFO** | First In First Out | Removes the oldest inserted data first, regardless of access | Simple queues, ordered data |
| **TTL** | Time To Live | Removes data after a set time expires, whether accessed or not | Time-sensitive data (stock prices, session tokens) |
| **Random** | Random Replacement | Removes a random entry | When access patterns are unpredictable |
| **ARC** | Adaptive Replacement Cache | Self-tuning hybrid of LRU + LFU, adapts to workload | High-performance systems needing smart eviction |

### Simple Examples

**LRU (Most Common in Data Engineering):**
```
Cache holds 3 items: [Dashboard_A, Dashboard_B, Dashboard_C]
User requests Dashboard_D → cache full
LRU evicts: Dashboard_A (accessed longest ago)
Cache is now: [Dashboard_B, Dashboard_C, Dashboard_D]
```

**TTL (Most Common for Freshness Control):**
```
Cache stores query result with TTL = 1 hour
At 9:00 AM → result cached
At 10:00 AM → TTL expires → cache evicts automatically
At 10:01 AM → next request re-fetches fresh result from DB
```

### Which Policy Is Used Where?

| Tool | Default Eviction Policy |
|------|------------------------|
| **Redis** | LRU (configurable: LFU, TTL, Random) |
| **Memcached** | LRU |
| **Python lru_cache** | LRU |
| **Snowflake Result Cache** | TTL (24 hours) |
| **Streamlit @st.cache_data** | TTL (configurable) |
| **CPU L1/L2 Cache** | LRU / Pseudo-LRU |

### Interview Tip
> "LRU is the most widely used eviction policy because it works well for most real-world access patterns where recently accessed data is likely to be accessed again soon."

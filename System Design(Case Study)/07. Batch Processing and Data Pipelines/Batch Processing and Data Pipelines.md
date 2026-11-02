Your product generates billions of clicks, orders, page views, and errors each day. How do systems like Datadog or a large analytics platform turn those raw events into useful dashboards, metrics, and models? The answer is a **data pipeline**, a system that produces derived data rather than a response to one user request.

Most pipelines follow the same stages: ingest, buffer, process, store, and serve. 

We still need to choose how quickly each consumer needs its result. 
- Batch processing handles bounded chunks at high throughput with minutes or hours of latency. 
- Stream processing handles an unbounded flow continuously and can produce results within seconds, but it is harder to operate correctly. 

Good pipelines use replayable stages, distinguish event time from processing time, support backfills, and avoid paying for streaming where a scheduled batch is enough.

```mermaid
graph LR
    AppServers[App servers] --> EventLog([Event log])
    EventLog --> BatchJob["Batch job<br>(hourly rollups)"]
    EventLog --> StreamJob["Stream job<br>(live counters)"]
    BatchJob --> Warehouse[(Warehouse)]
    StreamJob --> Warehouse
    Warehouse --> Dashboards[Dashboards + APIs]
```

# The core idea
Transactional databases, often called **OLTP** systems for online transaction processing, handle many small, targeted reads and writes. 
Analytics has the opposite shape: fewer queries that may scan billions of rows. Running `GROUP BY` over last month's events on the production database can overwhelm the same database serving checkout. Give analytics its own path, with columnar, append-only storage partitioned by time and parallel compute that runs near the data.

Make two decisions early. First, choose **batch or streaming per consumer**. 
- Batch processes a bounded input such as yesterday's events. It is cheaper and easier to replay. 
- Streaming processes an unbounded input continuously and provides fresher results, but adds state, event-time handling, and exactly-once processing concerns. 
A fraud model may need results within seconds, while a finance report only needs a correct daily result.

Second, decide **where the source of truth lives**. Keep an immutable log or data lake of raw events and treat downstream tables as derived, rebuildable data. If a bug corrupts an aggregate, we can correct the logic and replay the raw input.

# 
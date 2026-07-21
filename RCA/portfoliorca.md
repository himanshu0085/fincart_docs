---

# Root Cause Analysis (RCA)

## fincart-portfolio Service – Portfolio API Failures due to Database Connection Pool Exhaustion

---

## Service Affected

**fincart-portfolio (Azure App Service)**

---

## Issue

The **fincart-portfolio** service experienced intermittent failures resulting in HTTP 500 responses, significantly increased response times, and degraded Portfolio API performance. During the incident, the service became unstable and required an App Service restart to restore normal operation.

---

## Environment

* Azure App Service
* Service: fincart-portfolio
* Database: PostgreSQL
* Kafka Consumer
* HikariCP Connection Pool

---

# Incident Window (Verified)

**20 July 2026 20:32 IST – 21 July 2026 00:30 IST**

Major degradation observed during:

* **20:32 IST** – First HikariCP connection timeout observed
* **21:40–22:30 IST** – Continuous database connection timeout spike
* **23:20–23:55 IST** – Severe connection pool exhaustion
* **23:52 IST** – Application restarted
* **00:30 IST** – Service recovered

---

# 1. Executive Summary

On **20 July 2026**, the **fincart-portfolio** service started experiencing intermittent failures due to database connection exhaustion.

The earliest observable symptom was repeated **HikariCP connection acquisition timeout** beginning at **20:32:23 IST**.

Application logs consistently reported:

```
HikariPool-2
Connection is not available,
request timed out after 30000ms

total=10
active=10
idle=0
```

This indicates that all available database connections were occupied, preventing new application requests from acquiring a database connection.

As database connections became exhausted:

* Portfolio APIs became increasingly slow.
* API requests started timing out.
* HTTP 500 responses increased.
* Database operations became significantly slower.
* Kafka consumers exceeded the configured processing interval.
* Kafka consumers were repeatedly evicted from their consumer group.
* Message redelivery increased database load, creating a continuous failure loop.

Restarting the App Service recreated the database connection pool and Kafka consumer, temporarily restoring service.

---

# 2. Timeline

| Time (IST)        | Event                                                           |
| ----------------- | --------------------------------------------------------------- |
| **20:32:23**      | First HikariCP connection timeout observed.                     |
| **20:32–21:30**   | Intermittent database connection acquisition failures continue. |
| **21:40–22:30**   | Continuous HikariCP timeout spike observed.                     |
| **22:00–23:55**   | Database connection contention increases significantly.         |
| **23:20–23:55**   | Portfolio APIs increasingly return HTTP 500 responses.          |
| **23:52**         | Application restarted.                                          |
| **00:02 onwards** | Kafka CommitFailedException observed.                           |
| **~00:30**        | Service stabilized after restart.                               |

---

# 3. Investigation

## 3.1 Response Time Degradation

Azure Metrics showed repeated spikes in **Average Response Time** during the incident window, indicating severe application degradation before the service restart.

> **Insert Figure 1 – Azure Metrics (Average Response Time)**

<img width="1828" height="852" alt="image" src="https://github.com/user-attachments/assets/1038f19b-d58a-4729-ae7c-e7906409cbfe" />


---

## 3.2 Database Connection Pool Exhaustion

Grafana Loki logs showed repeated HikariCP connection acquisition failures throughout the incident.

The first observed timeout occurred at **20:32:23 IST**, confirming the beginning of the incident.

Representative log:

```
HikariPool-2

Connection is not available,
request timed out after 30000ms

total=10
active=10
idle=0
waiting=1
```

> **Insert Figure 2 – First HikariCP timeout (20:32:23 IST)**
<img width="1675" height="855" alt="image" src="https://github.com/user-attachments/assets/0812083c-02c1-47ca-9ce2-c9fe5b1da3ee" />


---

As the incident progressed, connection contention increased significantly.

During peak degradation, HikariCP reported:

```
total=10
active=10
idle=0
waiting=14
```

This indicates:

* Every configured database connection was occupied.
* No idle connections were available.
* Fourteen application requests were simultaneously waiting for a database connection.
* Requests timed out after 30 seconds.

<img width="889" height="464" alt="image" src="https://github.com/user-attachments/assets/73b744d1-0df2-43e8-9e60-5e3222ad020b" />


---

The Grafana timeline confirms that HikariCP connection acquisition failures continued throughout the evening, with major spikes observed between **21:40–22:30 IST** and **23:20–23:55 IST**.

<img width="1077" height="153" alt="image" src="https://github.com/user-attachments/assets/38b90816-cde5-4017-a15e-c4dbcb96221b" />

---

## 3.3 Slow Database Operations

Application logs showed repeated slow executions of the **insertTrxnsData** operation.

Execution times ranged between approximately **7 seconds and 16 seconds**, considerably longer than expected.

These long-running operations retained database connections for extended periods, reducing the number of available connections for incoming requests.

<img width="1123" height="824" alt="image" src="https://github.com/user-attachments/assets/06f9107a-6609-4b1f-add6-eef7fc6bee2b" />


---

## 3.4 Kafka Consumer Failures

As database processing became increasingly slow, Kafka consumers exceeded the configured **max.poll.interval.ms**.

This resulted in repeated:

* CommitFailedException
* Consumer group eviction
* Consumer rebalancing
* Message redelivery

Repeated message processing further increased database activity, worsening connection pool exhaustion.

<img width="1920" height="1080" alt="Screenshot from 2026-07-21 11-15-30" src="https://github.com/user-attachments/assets/22a691f0-0698-44d3-af83-657b2a360bce" />


---

# 4. Root Cause

The investigation indicates that the incident was caused by **database connection pool exhaustion resulting from prolonged database operations**.

Slow-running database operations retained all available HikariCP database connections for extended durations.

Consequently:

* All configured database connections became occupied (`active=10`).
* No idle database connections remained (`idle=0`).
* Incoming application requests waited for database connections.
* Connection acquisition timed out after 30 seconds.
* Portfolio APIs returned HTTP 500 responses.
* Kafka consumers exceeded the configured processing interval.
* Kafka generated repeated `CommitFailedException`.
* Consumer rebalancing caused repeated message processing.
* Additional database load further increased connection contention, creating a self-sustaining failure loop.

The issue persisted until the App Service was restarted, which recreated the HikariCP connection pool and Kafka consumer.

---

# 5. Impact

The incident resulted in:

* Portfolio APIs returning intermittent HTTP 500 responses.
* Significant increase in API response time.
* Portfolio calculation failures.
* Kafka consumer instability.
* Repeated consumer group rebalancing.
* Increased message processing latency.
* Service degradation requiring manual App Service restart.

---

# 6. Recommended Remediation

## Immediate

* Restart App Service to restore HikariCP connection pool.
* Verify database health during incident windows.
* Monitor active database connections.

---

## Short-Term

* Investigate slow `insertTrxnsData` execution.
* Review missing indexes.
* Investigate database locking and blocking.
* Review HikariCP configuration.
* Validate whether connection pool size is appropriate.
* Increase Kafka `max.poll.interval.ms` if necessary.

---

## Long-Term

* Move database write operations off the Kafka polling thread.
* Implement asynchronous processing for database writes.
* Add monitoring and alerting for:

  * Active database connections
  * Idle connections
  * Waiting threads
  * Kafka consumer lag
  * Kafka rebalance events
  * Slow database queries
* Implement idempotent Kafka message processing.

---

# Conclusion

The investigation confirms that the **immediate technical failure** was **HikariCP database connection pool exhaustion**, caused by prolonged database operations that retained all available connections. As the connection pool became exhausted, Portfolio API requests timed out, resulting in HTTP 500 responses. The resulting delays also impacted Kafka consumer processing, causing repeated consumer rebalances and message redelivery, which further increased database load. Restarting the App Service temporarily restored normal operation by recreating the database connection pool and Kafka consumer.

---


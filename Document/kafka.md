# Confluent Cloud Kafka: Basic vs Standard vs Enterprise – Limits, Throughput, Storage, and Operational Characteristics

## 1. Overview

Confluent Cloud is a fully managed Apache Kafka service that enables real-time data streaming pipelines and event-driven applications without managing infrastructure.

Confluent Cloud provides multiple cluster types based on scalability, performance, availability, and enterprise features:

| Cluster Type | Target Use Case |
|--------------|----------------|
| Basic | Development, testing, experimentation, and low-throughput workloads |
| Standard | Production workloads with moderate throughput and availability requirements |
| Enterprise | Mission-critical, high-throughput production workloads requiring enhanced scalability, governance, and networking |
| Dedicated | Highest throughput and isolated single-tenant workloads |

Each cluster operates independently with its own partitions, throughput capacity, storage, and configuration.

Reference:  
https://docs.confluent.io/cloud/current/clusters/cluster-types.html

---

## 2. Number of Clusters per Plan

Confluent Cloud does not define a fixed hard limit on the number of clusters per account based solely on Basic, Standard, or Enterprise plans.

Cluster creation is governed by:

- Organization quotas
- Account limits
- Available Confluent Kafka Units (CKUs or eCKUs)
- Billing configuration
- Region availability

Users can create multiple clusters under any plan, subject to these quotas.

Reference:  
https://docs.confluent.io/cloud/current/clusters/cluster-types.html

---

## 3. Partition Limits and Included Partitions

Partitions enable parallelism, scalability, and throughput in Kafka.

| Plan | Included Partitions | Billing Beyond Limit |
|------|--------------------|----------------------|
| Basic | 10 partitions | Additional partitions may incur charges |
| Standard | 500 partitions | Additional partitions billed |
| Enterprise | No partition-based limits | No partition-specific billing |

Reference:  
https://docs.confluent.io/cloud/current/billing/overview.html

---

## 3.1 40% Capacity Cap (Basic and Standard Only)

Basic and Standard clusters include partitions subject to a 40% cluster utilization cap.

This cap refers to overall cluster utilization, including:

- Ingress throughput
- Egress throughput
- Connection count
- Request rate
- Replication workload

When utilization exceeds this threshold:

- Producer requests may be throttled
- Consumer lag may increase
- Quota exceeded errors may occur

Enterprise clusters do not have this 40% cap.

Reference:  
https://docs.confluent.io/cloud/current/quotas/overview.html

---

## 4. Topic Limits

There is no strict hard limit on the number of topics.

Practical limits depend on:

- Partition count
- Cluster capacity
- Quotas

Reference:  
https://docs.confluent.io/cloud/current/topics/topics-faq.html

---

## 5. Throughput Guidelines

Confluent provides recommended throughput guidelines based on cluster type.

These values represent baseline throughput recommendations.

| Plan | Ingress Throughput | Egress Throughput |
|------|-------------------|------------------|
| Basic | 250 MB/s | 750 MB/s |
| Standard | 250 MB/s | 750 MB/s |
| Enterprise | 1,920 MB/s | 5,760 MB/s |

Reference:  
https://www.confluent.io/confluent-cloud/pricing/

---

## 5.1 Throughput Is Not a Hard Limit

Actual throughput depends on multiple factors:

### CKU Allocation
Higher CKUs increase throughput capacity.

Reference:  
https://docs.confluent.io/cloud/current/clusters/cluster-types.html

### Partition Count
Throughput scales with partition count.

Reference:  
https://docs.confluent.io/cloud/current/client-apps/optimizing/throughput.html

### Message Size
Larger messages affect throughput depending on batching and compression.

Reference:  
https://www.confluent.io/learn/kafka-message-size-limit/

### Load Distribution
Uniform partition distribution improves throughput efficiency.

---

## 6. Storage and Retention

| Plan | Storage Capacity | Durability |
|------|------------------|------------|
| Basic | Approximately 5 TB | Managed replication |
| Standard | Virtually unlimited | Managed replication |
| Enterprise | Virtually unlimited | Managed replication |

Reference:  
https://www.confluent.io/confluent-cloud/pricing/

---

## 6.1 Storage Behavior After 5 TB (Basic Plan)

Kafka uses retention policies instead of overwriting new data.

Kafka always follows FIFO retention logic:

- Oldest data is deleted first
- Newest data is preserved

Retention is controlled by:

Time-based retention:

retention.ms

Size-based retention:

retention.bytes


Reference:  
https://docs.confluent.io/platform/current/kafka/post-deployment.html#configure-log-retention

If retention is configured properly:

- Old data is deleted automatically
- New data continues to be accepted

If retention is unlimited and storage fills:

- Writes may be throttled
- Writes may fail
- Cluster upgrade may be required

Kafka never uses LIFO overwriting.

---

## 7. Producers and Consumers

Producer and consumer scaling depends on partitions.

- One partition supports one consumer thread
- Multiple partitions enable parallel processing

Reference:  
https://docs.confluent.io/platform/current/clients/consumer.html

Connection limits depend on:

- CKU allocation
- Cluster quotas

Reference:  
https://docs.confluent.io/cloud/current/quotas/overview.html

---

## 8. Networking and Security

| Feature | Basic | Standard | Enterprise |
|--------|-------|----------|------------|
| Public access | Yes | Yes | Yes |
| Private networking | No | Yes | Yes |
| RBAC | Limited | Full | Full |

Reference:  
https://docs.confluent.io/cloud/current/networking/overview.html

---

## 9. Cluster-to-Cluster Data Transfer (Cluster Linking)

Cluster Linking enables replication between Kafka clusters.

Reference:  
https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html

### 9.1 What Is Replicated

Cluster Linking replicates:

- Topic data
- Partitions
- Offsets
- Topic configuration

Reference:  
https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/overview.html

---

### 9.2 Throughput of Cluster Linking

Cluster Linking throughput depends on:

- Source cluster egress capacity
- Destination cluster ingress capacity
- Partition count
- CKU allocation

Enterprise clusters support significantly higher throughput.

Reference:  
https://docs.confluent.io/cloud/current/client-apps/optimizing/throughput.html

---

### 9.3 Billing Model

Cluster Linking is billed based on:

- Data egress from source
- Data ingress into destination

Reference:  
https://docs.confluent.io/cloud/current/billing/overview.html

---

## 10. Message Size Limits

| Plan | Maximum Message Size |
|------|---------------------|
| Basic | 8 MB |
| Standard | 8 MB |
| Enterprise | Up to 20 MB |

Reference:  
https://www.confluent.io/learn/kafka-message-size-limit/

---

## 11. Durability and Replication

Confluent Cloud automatically replicates data across brokers.

Reference:  
https://docs.confluent.io/cloud/current/clusters/architecture.html

---

## 12. Summary Comparison

| Feature | Basic | Standard | Enterprise |
|--------|-------|----------|------------|
| SLA | No SLA | 99.9% SLA | 99.99% SLA |
| Included partitions | 10 | 500 | Unlimited |
| Throughput | Medium | Medium | Very high |
| Storage | ~5 TB | Unlimited | Unlimited |
| Cluster linking | Yes | Yes | Yes |
| Networking | Basic | Advanced | Enterprise |

---

# Official References

Cluster types:  
https://docs.confluent.io/cloud/current/clusters/cluster-types.html

Pricing and throughput guidelines:  
https://www.confluent.io/confluent-cloud/pricing/

Partition billing:  
https://docs.confluent.io/cloud/current/billing/overview.html

Quotas:  
https://docs.confluent.io/cloud/current/quotas/overview.html

Cluster linking:  
https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html

Retention and storage:  
https://docs.confluent.io/platform/current/kafka/post-deployment.html

Throughput optimization:  
https://docs.confluent.io/cloud/current/client-apps/optimizing/throughput.html

Kafka architecture:  
https://docs.confluent.io/cloud/current/clusters/architecture.html

# Confluent Cloud Kafka: Basic vs Standard vs Enterprise — Limits, Throughput, Storage, and Operational Characteristics

## 1. Overview

Confluent Cloud is a fully managed Apache Kafka service that enables real-time data streaming pipelines and event-driven applications without managing infrastructure.

Confluent Cloud provides multiple cluster types:

| Cluster Type | Target Use Case |
|-------------|----------------|
| Basic | Development, testing, experimentation, and low-throughput workloads |
| Standard | Production workloads with moderate throughput and availability requirements |
| Enterprise | Mission-critical, high-throughput production workloads requiring enhanced scalability, governance, and networking |
| Dedicated | Highest throughput and isolated single-tenant workloads |

Reference:  
https://docs.confluent.io/cloud/current/clusters/cluster-types.html

---

## 2. Number of Clusters per Plan

Confluent Cloud does not define a fixed hard limit on the number of clusters per account based solely on Basic, Standard, or Enterprise plans.

Cluster creation depends on:

- Organization quotas  
- Account limits  
- Available CKUs or eCKUs  
- Billing configuration  
- Region availability  

Reference:  
https://docs.confluent.io/cloud/current/clusters/cluster-types.html

---

## 3. Partition Limits and Included Partitions

| Plan | Included Partitions | Billing Beyond Limit |
|------|--------------------|----------------------|
| Basic | 10 partitions | Additional partitions may incur charges |
| Standard | 500 partitions | Additional partitions billed |
| Enterprise | No partition-based limits | No partition-specific billing |

Reference:  
https://docs.confluent.io/cloud/current/billing/overview.html

---

## 3.1 40% Capacity Cap

Basic and Standard clusters include partitions subject to a **40% cluster utilization cap**.

This includes:

- Ingress throughput  
- Egress throughput  
- Connections  
- Request rate  
- Replication load  

Reference:  
https://docs.confluent.io/cloud/current/quotas/overview.html

---

## 4. Throughput Guidelines

| Plan | Ingress Throughput | Egress Throughput |
|------|--------------------|-------------------|
| Basic | 250 MB/s | 750 MB/s |
| Standard | 250 MB/s | 750 MB/s |
| Enterprise | 1,920 MB/s | 5,760 MB/s |

Reference:  
https://www.confluent.io/confluent-cloud/pricing/

Throughput depends on:

- CKU allocation  
- Partition count  
- Message size  
- Load distribution  
- Network latency  

Reference:  
https://docs.confluent.io/cloud/current/client-apps/optimizing/throughput.html

---

## 5. Storage and Retention

| Plan | Storage Capacity | Durability |
|------|------------------|------------|
| Basic | Approximately 5 TB | Managed replication |
| Standard | Unlimited | Managed replication |
| Enterprise | Unlimited | Managed replication |

Reference:  
https://www.confluent.io/confluent-cloud/pricing/

Kafka uses FIFO retention and deletes oldest data first.

Reference:  
https://docs.confluent.io/platform/current/kafka/post-deployment.html

---

## 6. Cluster Linking

Cluster Linking enables replication between clusters.

Reference:  
https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html

Replication throughput depends on:

- Source egress  
- Destination ingress  
- Partition count  
- CKU allocation  

Reference:  
https://docs.confluent.io/cloud/current/client-apps/optimizing/throughput.html

---

## 7. Message Size Limits

| Plan | Maximum Message Size |
|------|----------------------|
| Basic | 8 MB |
| Standard | 8 MB |
| Enterprise | Up to 20 MB |

Reference:  
https://www.confluent.io/learn/kafka-message-size-limit/

---

## 8. Networking and Security

| Feature | Basic | Standard | Enterprise |
|---------|-------|----------|------------|
| Public networking | Yes | Yes | Yes |
| Private networking | No | Yes | Yes |
| RBAC | Limited | Full | Full |

Reference:  
https://docs.confluent.io/cloud/current/networking/overview.html

---

## Official References

- https://docs.confluent.io/cloud/current/clusters/cluster-types.html  
- https://www.confluent.io/confluent-cloud/pricing/  
- https://docs.confluent.io/cloud/current/billing/overview.html  
- https://docs.confluent.io/cloud/current/quotas/overview.html  
- https://docs.confluent.io/cloud/current/multi-cloud/cluster-linking/index.html  
- https://docs.confluent.io/platform/current/kafka/post-deployment.html  
- https://docs.confluent.io/cloud/current/client-apps/optimizing/throughput.html  

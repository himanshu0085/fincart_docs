# Apache Kafka® & Apache Flink® on Confluent Cloud™

## Azure Native ISV Service – Cost Structure, Pricing & Service Quotas

---

## 1. Introduction

**Apache Kafka® & Apache Flink® on Confluent Cloud™** is an **Azure Native ISV Service** available through **Azure Marketplace**. It provides a fully managed, cloud-native event streaming and stream processing platform that integrates directly with Azure services and billing.

Key characteristics:

* Fully managed by Confluent
* Natively integrated with Azure
* Billing consolidated into the Azure invoice
* No infrastructure management required by customers

Official overview:

* Azure documentation: [https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview](https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview)

---

## 2. Deployment & Billing Model on Azure

### 2.1 Azure Marketplace Integration

When deployed via **Azure Marketplace**, Confluent Cloud behaves as a SaaS resource:

* The service is provisioned from Azure Marketplace
* Resource appears under the Azure subscription
* Usage is metered and billed through Azure Cost Management

Marketplace listing:

* [https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod](https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod)

### 2.2 Azure Billing Flow

* No upfront license cost
* Usage-based billing
* Charges appear in:

  * Azure Portal → Cost Management → Cost analysis
* Publisher name: **Confluent**

Official reference:

* [https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview#billing](https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview#billing)

---

## 3. Cost Structure & Pricing Model

### 3.0 Payment & Billing Options on Azure

Apache Kafka® & Apache Flink® on Confluent Cloud™ as an **Azure Native ISV Service** supports **two official payment models** through Azure Marketplace:

1. **Pay-As-You-Go (PAYG)**
2. **Annual / Committed Spend (Advance Commitment)**

Both models are fully integrated with **Azure billing** and appear on the **Azure invoice**.

Official Azure billing confirmation:

* [https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview#billing](https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview#billing)

---

### 3.1 Pay-As-You-Go (PAYG) Model

**How it works:**

* No upfront or advance payment
* Usage is metered continuously based on **Confluent Consumption Units (CCUs)**
* Monthly billing through Azure invoice
* Full flexibility to scale up or down

**Best suited for:**

* Development and testing environments
* Proof of Concept (POC)
* Variable or unpredictable workloads

Official references:

* [https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview#billing](https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview#billing)
* [https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod](https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod)

---

### 3.2 Annual / Committed Spend Model (Advance Payment)

**How it works:**

* Customer commits to a **monthly or annual minimum spend**
* Commitment amount is consumed against actual CCU usage
* Generally provides **discounted rates** compared to PAYG
* Any usage beyond the committed amount is billed at standard PAYG rates
* Billing continues through the Azure invoice

**Important clarifications:**

* This is **not a fixed per-cluster price**
* The commitment is **financial**, not a resource lock
* Kafka and Flink usage remain fully elastic

**Best suited for:**

* Production workloads
* Stable and predictable traffic patterns
* Enterprise and long-running platforms

Official references:

* [https://www.confluent.io/confluent-cloud/pricing/](https://www.confluent.io/confluent-cloud/pricing/)
* [https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod](https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod)

---

### 3.3 Summary of Payment Options

| Aspect          | Pay-As-You-Go         | Annual / Commit                      |
| --------------- | --------------------- | ------------------------------------ |
| Upfront payment | No                    | Yes                                  |
| Billing         | Monthly Azure invoice | Azure invoice with commit adjustment |
| Discount        | No                    | Yes                                  |
| Flexibility     | High                  | Medium                               |
| Typical usage   | Dev / POC             | Production / Enterprise              |

---

## 3.4 Confluent Consumption Units (CCUs)

### 3.1 Confluent Consumption Units (CCUs)

Pricing is based on **Confluent Consumption Units (CCUs)**.

* 1 CCU ≈ **$0.01 USD**
* All usage (Kafka, Flink, storage, throughput) is converted into CCUs
* Total monthly cost = Total CCUs consumed × $0.01

Official pricing explanation:

* [https://www.confluent.io/confluent-cloud/pricing/](https://www.confluent.io/confluent-cloud/pricing/)

### 3.2 What Contributes to Cost

Primary cost drivers include:

**Apache Kafka®**

* Data ingress (producer throughput)
* Data egress (consumer throughput)
* Data retention & storage
* Number of partitions

**Apache Flink®**

* Flink compute pool size
* Query runtime duration
* State size

**Optional Services**

* Managed connectors
* Schema Registry
* Governance & security features

Billing FAQ:

* [https://docs.confluent.io/cloud/current/billing/billing-faq.html](https://docs.confluent.io/cloud/current/billing/billing-faq.html)

---

## 4. One Kafka Cluster – Cost Estimation (Azure Context)

> Note: Confluent Cloud does **not** have a fixed "per-cluster" price. Cost depends entirely on usage.

### 4.1 Development / Low Usage Cluster

Typical characteristics:

* Low throughput
* Short data retention
* Non-critical workloads

Estimated usage:

* ~1,000–2,000 CCUs per day

Estimated cost:

* $10–$20 per day
* $300–$600 per month

Reference example:

* [https://www.confluent.io/blog/confluent-cloud-managed-kafka-service-azure-marketplace/](https://www.confluent.io/blog/confluent-cloud-managed-kafka-service-azure-marketplace/)

---

### 4.2 Production / Moderate Usage Cluster

Typical characteristics:

* Continuous data ingestion
* Moderate consumer load
* Production-grade availability

Estimated usage:

* ~5,000–10,000 CCUs per day

Estimated cost:

* $50–$100 per day
* $1,500–$3,000 per month

---

### 4.3 High-Throughput / Enterprise Cluster

Typical characteristics:

* High sustained throughput
* Large retention periods
* Multiple consumer groups
* Flink processing jobs

Estimated usage:

* 20,000+ CCUs per day

Estimated cost:

* $200+ per day
* $6,000+ per month

---

## 5. Azure Pricing Calculator – Usage Clarification

### 5.1 Important Limitation

The **Azure Pricing Calculator** does **not** show Confluent Cloud costs directly because:

* Confluent Cloud is a SaaS (ISV) offering
* Pricing is controlled by Confluent, not Azure compute meters

Azure Pricing Calculator:

* [https://azure.microsoft.com/pricing/calculator/](https://azure.microsoft.com/pricing/calculator/)

### 5.2 Recommended Usage of Azure Calculator

Azure Pricing Calculator should be used only for **supporting Azure services**, such as:

* Azure Functions / AKS / Virtual Machines (Kafka clients)
* Azure Storage (ADLS, backups, sinks)
* Azure Networking & Bandwidth

Kafka & Flink costs must be estimated using **Confluent CCUs**, not Azure calculator entries.

---

## 6. Service Quotas & Limitations (Confluent Cloud)

All limits are defined by Confluent Cloud and apply even when deployed via Azure Marketplace.

### 6.1 Kafka & Environment Quotas (Default)

* Kafka clusters per environment: **20**
* Kafka clusters per organization: **400**
* Environments per organization: **25**
* API keys per organization: **1,000**
* Users per organization: **1,000**

Official quota reference:

* [https://docs.confluent.io/cloud/current/quotas/service-quotas.html](https://docs.confluent.io/cloud/current/quotas/service-quotas.html)

---

### 6.2 Apache Flink® Quotas

* Flink compute pools per environment: **50**
* Flink statements per region: **10,000**
* Flink state size:

  * Soft limit: **500 GB**
  * Hard limit: **1 TB**

Official Flink quota documentation:

* [https://docs.confluent.io/cloud/current/quotas/service-quotas.html#flink-quotas](https://docs.confluent.io/cloud/current/quotas/service-quotas.html#flink-quotas)

---

### 6.3 Quota Increase Policy

* Quotas are **default limits**
* Increases can be requested through Confluent Support
* Approval depends on region, plan, and workload

Quota overview:

* [https://docs.confluent.io/cloud/current/quotas/overview.html](https://docs.confluent.io/cloud/current/quotas/overview.html)

---

## 7. Cost Monitoring & Governance in Azure

After deployment:

* Azure Portal → Cost Management → Cost analysis
* Filter by:

  * Subscription
  * Resource group
  * Publisher: **Confluent**

You can:

* Set budget alerts
* Track daily/monthly spend
* Export cost reports

Azure cost management documentation:

* [https://learn.microsoft.com/en-us/azure/cost-management-billing/](https://learn.microsoft.com/en-us/azure/cost-management-billing/)

---

## 8. Conclusion

* Confluent Cloud on Azure follows a **pure usage-based pricing model**
* No fixed per-cluster price
* Costs scale with throughput, retention, and processing
* Azure Pricing Calculator does **not** calculate Kafka/Flink cost directly
* All charges appear transparently in Azure billing

This makes the service well-suited for scalable, production-grade, event-driven architectures on Azure without operational overhead.

---

## 9. Key Official References (Summary)

* Azure Overview: [https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview](https://learn.microsoft.com/en-us/azure/partner-solutions/apache-kafka-confluent-cloud/overview)
* Azure Marketplace Listing: [https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod](https://marketplace.microsoft.com/en-us/product/saas/confluentinc.confluent-cloud-azure-prod)
* Confluent Pricing: [https://www.confluent.io/confluent-cloud/pricing/](https://www.confluent.io/confluent-cloud/pricing/)
* Billing FAQ: [https://docs.confluent.io/cloud/current/billing/billing-faq.html](https://docs.confluent.io/cloud/current/billing/billing-faq.html)
* Service Quotas: [https://docs.confluent.io/cloud/current/quotas/service-quotas.html](https://docs.confluent.io/cloud/current/quotas/service-quotas.html)
* Azure Cost Management: [https://learn.microsoft.com/en-us/azure/cost-management-billing/](https://learn.microsoft.com/en-us/azure/cost-management-billing/)

# Azure CDN (Classic) – Pre‑Migration Inventory Document

## Purpose

This document captures the **current-state configuration** of the Azure CDN (Classic) setup.
No changes were performed. This inventory is created **before approval and before any migration activity** to Azure Front Door.

> Guiding principle: *If it’s not documented, it doesn’t exist.*

---

## 1. CDN Profile Details

* **CDN Profile Name:** `3eadvisors-501522a9ec-cdnprofile`
* **Service Type:** Microsoft CDN (Classic)
* **Status:** Active / Running
* **Scope:** Global

---

## 2. Endpoint Details

* **Endpoint Name:** `3eadvisors-501522a9ec-endpoint`
* **Endpoint Hostname:**
  `3eadvisors-501522a9ec-endpoint.azureedge.net`
* **Protocols Enabled:** HTTP, HTTPS
* **Optimization Type:** General web delivery

---

## 3. Origin Configuration

### 3.1 Configured Origins

| Origin Name            | Origin Type    | Origin Hostname                            | Protocols   | Ports   | Status  |
| ---------------------- | -------------- | ------------------------------------------ | ----------- | ------- | ------- |
| 3eadvisors-wordpress-* | Web App        | 3eadvisors-wordpress-*                     | HTTP, HTTPS | 80, 443 | Enabled |
| 3eadvisors501522a9ec-* | Storage (Blob) | 3eadvisors501522a9ec.blob.core.windows.net | HTTP, HTTPS | 80, 443 | Enabled |

### 3.2 Origin Groups

| Origin Group Name | Default | Notes                        |
| ----------------- | ------- | ---------------------------- |
| blob-origin-group | Yes     | Used for blob-based content  |
| app-origin-group  | No      | Used for application routing |

---

## 4. Custom Domains

* **Custom Domains Configured:** None
* **Custom HTTPS:** Not applicable

> The endpoint currently serves traffic only via the default `azureedge.net` hostname.

---

## 5. Rules Engine Configuration

### Rule Name: `originOverrideRule`

**Condition**

* If **URL path** does **NOT begin with** `blob3eadvisors501...`
* Case transform: Lowercase

**Actions**

1. **Origin Group Override**

   * Target origin group: `app-origin-group`

2. **URL Rewrite**

   * Source pattern: `/blob3eadvisors501...`
   * Destination: `/`
   * Preserve unmatched path: Yes

3. **Modify Response Header**

   * Action: Overwrite
   * Header name: `Access-Control-Allow-Origin`
   * Header value: `https://3eadvisors...`

> This rule controls routing between blob storage and application backend and also enforces CORS behavior.

---

## 6. Compression Settings

* **Compression:** Enabled
* **Formats Enabled (sample):**

  * application/javascript
  * application/json
  * application/xml
  * application/font-*
  * application/xhtml+xml
  * application/rss+xml

> Compression is enabled to reduce payload size and improve performance.

---

## 7. Security & Access

* **WAF:** Not configured at CDN Classic level
* **Geo-filtering:** Not configured
* **IP restrictions:** Not configured

---

## 8. Activity Scope

* This document is based on **read-only review** of the Azure Portal.
* **No changes** were made to:

  * CDN profile
  * Endpoint
  * Origins
  * Rules
  * DNS

---

## 9. Migration Readiness Summary

* CDN profile is **Azure CDN (Classic)** and subject to future retirement.
* Configuration includes:

  * Multiple origins (Web App + Blob Storage)
  * Rules-based routing using Rules Engine
  * Compression enabled
  * No custom domains

This setup is **ready for parallel migration** to **Azure Front Door (Standard)** once approval is received.

---

## 10. Target Architecture – Azure Front Door (Standard)

This section defines how the existing Azure CDN (Classic) configuration will be mapped to Azure Front Door (Standard).

### 10.1 Front Door Profile

* **Service:** Azure Front Door
* **Tier:** Standard
* **Deployment Model:** Parallel (existing CDN remains active during setup)

---

### 10.2 Origin Mapping

| Current CDN Origin                         | Type         | Azure Front Door Equivalent         |
| ------------------------------------------ | ------------ | ----------------------------------- |
| 3eadvisors-wordpress-*                     | Web App      | Front Door Origin (App Service)     |
| 3eadvisors501522a9ec.blob.core.windows.net | Blob Storage | Front Door Origin (Storage Account) |

---

### 10.3 Origin Groups Mapping

| CDN Origin Group  | Front Door Origin Group | Purpose               |
| ----------------- | ----------------------- | --------------------- |
| blob-origin-group | fd-blob-origin-group    | Static / blob content |
| app-origin-group  | fd-app-origin-group     | Application traffic   |

Health probes and priorities will be configured to match existing behavior.

---

### 10.4 Routing & Rules Mapping

| CDN Rule                     | Front Door Rules Engine Equivalent |
| ---------------------------- | ---------------------------------- |
| URL path condition           | Match condition: URL path          |
| Origin group override        | Route to specific origin group     |
| URL rewrite                  | URL rewrite action                 |
| Response header modification | Modify response header action      |

> Rules will be recreated manually in Azure Front Door Rules Engine.

---

### 10.5 Compression Mapping

* Azure CDN Compression: **Enabled**
* Azure Front Door Compression: **Enabled**
* MIME types will be aligned to current configuration

---

### 10.6 Custom Domains & HTTPS

* Current state: No custom domains
* Target state:

  * Use default `*.azurefd.net` hostname initially
  * Custom domains (if required) to be added post-validation
  * Azure-managed certificates recommended

---

### 10.7 Cutover Strategy (High-Level)

1. Deploy Azure Front Door (Standard) in parallel
2. Configure origins, origin groups, routes, and rules
3. Validate using `*.azurefd.net` endpoint
4. Perform DNS cutover (when approved)
5. Monitor and rollback via DNS if required

---

### 10.8 Risk & Rollback Plan

* **Risk Level:** Low (parallel deployment)
* **Rollback Method:** DNS revert to CDN Classic endpoint
* **Estimated Rollback Time:** < 10 minutes (TTL dependent)

---

**Document Status:** Final – Inventory Complete & Target Mapping Defined

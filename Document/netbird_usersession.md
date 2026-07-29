# Explore NetBird User Session Tracking and Access Auditing Capabilities

## Objective

Evaluate whether **NetBird** natively provides capabilities for:

* User session tracking
* User access auditing

This assessment is based **only on the official NetBird documentation** and considers **native product capabilities**. External integrations, custom scripts, APIs for custom reporting, or SIEM solutions are **not** considered as fulfilling the requirements.

---

# 1. User Session Tracking

## Requirement

Investigate whether NetBird provides logs for:

* User login time
* User logout time
* Session duration

Verify whether this information is available through the NetBird Dashboard and Audit Logs.

## Findings

NetBird provides an **Audit Events** page in the Dashboard that records management and authentication-related activities. The documented events include **Dashboard Login** and **User Logged In Peer**, allowing administrators to identify when a user authenticated.

However, the official documentation does **not** document:

* User logout events
* Dashboard logout events
* VPN disconnect events
* Session duration tracking

### Capability Assessment

| Requirement      | Status            | Remarks                                                                                   |
| ---------------- | ----------------- | ----------------------------------------------------------------------------------------- |
| User login time  | **Supported**     | NetBird records login events through the Audit Events page.                               |
| User logout time | **Not Supported** | NetBird does not provide a native user logout event.                                      |
| Session duration | **Not Supported** | NetBird does not track or report session duration.                                        |
| Audit Events     | **Supported**     | NetBird provides an Audit Events page in the Dashboard to view recorded audit activities. |

### Conclusion

NetBird supports **user login auditing**, but it **does not provide complete user session tracking**, as logout events and session duration are not available.

---

# 2. User Access Auditing

## Requirement

Prepare an environment-wise user access report showing:

* Users with access to **Production, Staging, and UAT**
* Date when access was granted
* Date when access was disabled/revoked

## Findings

NetBird provides **Users**, **Groups**, and **Access Policies** for access management. However, based on the official documentation, NetBird does **not** provide a built-in report that displays environment-wise user access or historical access information for specific environments such as Production, Staging, or UAT.

### Capability Assessment

| Requirement                                           | Status            | Remarks                                                                                                  |
| ----------------------------------------------------- | ----------------- | -------------------------------------------------------------------------------------------------------- |
| Users with access to **Production, Staging, and UAT** | **Not Supported** | NetBird does not provide a built-in environment-wise user access report.                                 |
| Date access was granted                               | **Not Supported** | NetBird does not provide a native report showing when access was granted for an environment.             |
| Date access was disabled/revoked                      | **Not Supported** | NetBird does not provide a native report showing when access was disabled or revoked for an environment. |

### Conclusion

NetBird does **not** provide native environment-wise access auditing. Reports showing users with access to Production, Staging, or UAT, along with access grant and revocation dates, are **not available** as built-in features.

---

# Overall Assessment

| Capability                          | Status            |
| ----------------------------------- | ----------------- |
| User login tracking                 | **Supported**     |
| User logout tracking                | **Not Supported** |
| Session duration tracking           | **Not Supported** |
| Audit Events                        | **Supported**     |
| Environment-wise user access report | **Not Supported** |
| Access granted report               | **Not Supported** |
| Access revoked report               | **Not Supported** |

## References

* **NetBird Documentation – Audit Events:** [https://docs.netbird.io/manage/activity](https://docs.netbird.io/manage/activity)
* **NetBird Documentation – Audit Events API:** [https://docs.netbird.io/api/resources/events](https://docs.netbird.io/api/resources/events)

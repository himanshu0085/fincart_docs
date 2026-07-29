# Explore NetBird Device Binding and Microsoft Entra ID Authentication & User Creation Cost

## Objective

Evaluate:

1. Whether **NetBird** natively supports **device binding**.
2. Whether application access can be restricted to **Microsoft Entra ID (Azure AD)** authentication only.
3. The cost of creating users in **Microsoft Entra ID**.

This assessment is based solely on the **official NetBird** and **Microsoft Learn** documentation and evaluates **native product capabilities**.

---

# 1. NetBird Device Binding

## Requirement

* Ensure users can connect only from authorized devices.
* Review the available NetBird features for device binding.

## Findings

The official NetBird documentation identifies each connecting device as a **Peer**. However, it does **not** describe a native capability to:

* Bind a user account to a specific device.
* Restrict a user to a single registered device.
* Prevent a user from registering and using multiple devices.
* Enforce device binding during user authentication.

### Capability Assessment

| Requirement                                                          | Status            | Remarks                                                                                                                                                                                                     |
| -------------------------------------------------------------------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Device binding (allow users to connect only from authorized devices) | **Not Supported** | The official NetBird documentation does not provide a native device-binding capability that associates a user with a specific device or prevents the same user from registering and using multiple devices. |

### Conclusion

NetBird **does not natively support device binding** as defined in the requirement.

---

# 2. Microsoft Entra ID Authentication & User Creation Cost

## Requirement

* Verify whether application access can be restricted to authentication through **Microsoft Entra ID (Azure AD)** only.
* Explore the cost of creating users in Microsoft Entra ID, including:

  * Internal users
  * Guest users
  * External identities

## Findings

Microsoft Entra ID supports industry-standard authentication protocols such as **OpenID Connect (OIDC)**, **OAuth 2.0**, and **SAML 2.0**, allowing applications to authenticate users through Microsoft Entra ID.

Microsoft does **not** charge separately for creating user accounts. User creation is included with Microsoft Entra ID. Additional costs apply only when premium identity capabilities or licensing are required.

### Capability Assessment

| Requirement                                               | Status          | Remarks                                                                                                                                                                                                                          |
| --------------------------------------------------------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Restrict application authentication to Microsoft Entra ID | **Supported**   | Applications can be configured to authenticate users through Microsoft Entra ID using supported authentication protocols.                                                                                                        |
| Internal user creation                                    | **Free**        | Creating internal (member) users does not incur a separate user creation charge. Premium licensing is required only when premium Microsoft Entra features are used.                                                              |
| Guest user creation (B2B)                                 | **Free**        | Inviting or creating guest users does not incur a separate user creation charge.                                                                                                                                                 |
| External identities                                       | **Usage-based** | Microsoft Entra External ID follows a Monthly Active User (MAU) billing model. The Basic tier includes the first **50,000 Monthly Active Users (MAUs)** at no cost. Charges apply only after the free MAU allowance is exceeded. |

### Additional Licensing

Additional Microsoft Entra licensing is required only when using premium identity capabilities, such as:

* Conditional Access
* Identity Protection
* Privileged Identity Management (PIM)
* Identity Governance

These capabilities require the applicable Microsoft Entra premium licenses (for example, Microsoft Entra ID P1, P2, or Microsoft Entra Suite).

### Conclusion

Microsoft Entra ID supports application authentication and does **not** charge separately for creating internal or guest users. Costs are incurred only when premium Microsoft Entra capabilities are required or when Microsoft Entra External ID usage exceeds the included Monthly Active User (MAU) allowance.

---

# References

## NetBird

* NetBird Documentation – Peer Approval: [https://docs.netbird.io/manage/peers/approve-peers](https://docs.netbird.io/manage/peers/approve-peers)
* NetBird Documentation – Peers: [https://docs.netbird.io/manage/peers](https://docs.netbird.io/manage/peers)

## Microsoft

* Microsoft Learn – Create, invite, and delete users: [https://learn.microsoft.com/en-us/entra/fundamentals/how-to-create-delete-users](https://learn.microsoft.com/en-us/entra/fundamentals/how-to-create-delete-users)
* Microsoft Learn – Microsoft Entra licensing: [https://learn.microsoft.com/en-us/entra/fundamentals/licensing](https://learn.microsoft.com/en-us/entra/fundamentals/licensing)
* Microsoft Entra External ID Pricing: [https://www.microsoft.com/security/business/microsoft-entra-pricing](https://www.microsoft.com/security/business/microsoft-entra-pricing)

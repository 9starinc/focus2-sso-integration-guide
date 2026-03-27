# Focus2 Application  
# SAML 2.0 / Single Sign-On (SSO) Integration Guide

---

## Table of Contents

- [Overview](#overview)  
- [Information Customer Needs](#information-customer-needs)  
  - [Focus2 Service Provider Metadata](#1-focus2-service-provider-metadata)  
  - [Service Provider Core Values](#2-service-provider-core-values)  
  - [Required SAML Attributes](#3-required-saml-attributes)  
  - [NameID Requirements](#4-nameid-requirements)  
- [Information Needed From Customer](#information-needed-from-customer)  
- [Testing the Integration](#testing-the-integration)  
- [Microsoft Entra ID (Azure AD) Configuration Example](#microsoft-entra-id-azure-ad-configuration-example)  

---

## Overview

The **Focus2 application** supports authentication using **SAML 2.0 Single Sign-On (SSO)**.

In this integration:

- **Focus2** acts as the **Service Provider (SP)**  
- Your organization’s system acts as the **Identity Provider (IdP)**  

### Technical Requirements

The integration requires:

- SAML 2.0 Browser SSO profile  
- Signed SAML Responses  
- Encrypted SAML Assertions (if possible)
- Required user attributes (First Name, Last Name, Email)

---

## Information Customer Needs

### 1. Focus2 Service Provider Metadata

SP Metadata URL:

```
https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/metadata.php/fc-sp-proxy
```

This metadata contains:

- SP EntityID  
- Assertion Consumer Service (ACS) URL  
- Signing certificate  
- Encryption certificate  
- Single Logout endpoint (if configured)

---

### 2. Service Provider Core Values

| Setting | Value |
|----------|-------|
| SP EntityID | `https://fc.proxy.elasticsso.com/saml/sp` |
| ACS URL | `https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/saml2-acs.php/fc-sp-proxy` |
| Protocol | SAML 2.0 |
| Profile | Browser SSO |
| Binding | HTTP-POST |
| Sign Response | Required |
| Encrypt Assertion | Preferred |

---

### 3. Required SAML Attributes

| Logical Field | Expected SAML Attribute |
|---------------|-------------------|
| First Name | `urn:oid:2.5.4.42` |
| Last Name | `urn:oid:2.5.4.4` |
| Email Address | `urn:oid:0.9.2342.19200300.100.1.3` |

- Email must be unique per user.  
- Custom attribute names must be communicated during setup.

---

### 4. NameID Requirements

- NameID may be **Transient**, **EmailAddress**, or another unique identifier.  
- Email is typically used as the primary identifier.  

---

## Information Needed From Customer

### Option 1 – Identity Provider Metadata (Preferred)

Provide:

- Identity Provider metadata file  
  **OR**
- Identity Provider metadata URL  

### Option 2 – Manual Configuration Details

If metadata cannot be provided, supply:

- IdP EntityID  
- IdP signing certificate  
- IdP encryption certificate (if available)  
- Single Sign-On endpoint URL  
- SSO binding  
- Single Logout endpoint (if supported)

### Required Confirmation

Please confirm:

- SAML response is signed  
- Assertion is encrypted (if possible)
- Required attributes are included  
- Email is unique per user  

---

## Testing the Integration

Initiate login using:

```
https://fc.proxy.elasticsso.com/saml/module.php/elasticsso/sso.php?entityID=customer_idp_entityID
```

Replace:

```
customer_idp_entityID
```

With your Identity Provider’s EntityID.

---

## Microsoft Entra ID (Azure AD) Configuration Example

### Create the Enterprise Application

1. Log into https://entra.microsoft.com  
2. Navigate to **Identity → Applications → Enterprise applications**  
3. Select **+ New application**  
4. Choose **+ Create your own application**  
5. Name the application: `Focus2`  
6. Select **Integrate any other application you don't find in the gallery (Non-gallery)**  
7. Click **Create**

---

### Configure SAML Single Sign-On

1. Open the application  
2. Select **Single sign-on**  
3. Choose **SAML**

#### Basic SAML Configuration

| Field | Value |
|-------|-------|
| Identifier | `https://fc.proxy.elasticsso.com/saml/sp` |
| Reply URL | `https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/saml2-acs.php/fc-sp-proxy` |
| Sign-on URL | *Leave blank* |
| Relay State | *Leave blank* |
| Logout URL | `https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/saml2-logout.php/fc-sp-proxy` |

Save changes.

Provide the metadata URL to Focus2.

---

### Focus2 SP Certificate (Signing & Encryption)

The Focus2 Service Provider uses the following certificate for **both signing and encryption**:

```
-----BEGIN CERTIFICATE-----
MIIDnzCCAoegAwIBAgIJAPg+z9I0D7D/MA0GCSqGSIb3DQEBCwUAMGYxCzAJBgNV
BAYTAlVTMQ4wDAYDVQQIDAVUZXhhczEPMA0GA1UEBwwGQXVzdGluMRQwEgYDVQQK
DAs5U1RBUiwgSW5jLjEgMB4GA1UEAwwXZmMucHJveHkuZWxhc3RpY3Nzby5jb20w
HhcNMTgwMjE0MTYyMDAzWhcNMjgwMjE0MTYyMDAzWjBmMQswCQYDVQQGEwJVUzEO
MAwGA1UECAwFVGV4YXMxDzANBgNVBAcMBkF1c3RpbjEUMBIGA1UECgwLOVNUQVIs
IEluYy4xIDAeBgNVBAMMF2ZjLnByb3h5LmVsYXN0aWNzc28uY29tMIIBIjANBgkq
hkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAroNUH/4soyODhzlq9tB5EA92DkicpOve
GZL3b41t5Xhp+pugilnwy/9jiryuVhPrDLy5OMy0MTIPNEU/4i/vsRDvl2UNkRn6
SNP5JGZGHYyqd4YSK4VMgDtATgB7eSfQ6zq2ySIceZUIt/AaU4ykjCu5j7URlr53
Q+JPqU2I7yXX9kTbNL/CiQAL8JeUYIdJwtxp8sLPFwS4peaVI3tcX9RoJNb3/5mE
lAesbu7lcjHKlH2xx7hPrU6/kFqtRgWIreZSlUYoQeYMOmLh9Alj2E5bX8RANRfx
Ijc4ndQlgjrlm4+1hyrA7F6AnKSl/z+oddRXdNBBdAg2TkQBfmCsQwIDAQABo1Aw
TjAdBgNVHQ4EFgQU49hkhmUKkPMU09jBJMJr7zwB6SYwHwYDVR0jBBgwFoAU49hk
hmUKkPMU09jBJMJr7zwB6SYwDAYDVR0TBAUwAwEB/zANBgkqhkiG9w0BAQsFAAOC
AQEAVmWomBEDltABEO83bvTNGC4Ptq1Ib9KPVuetAwioRKtdrl+d8YGvkAXDSw2v
pJXtuun0bfFRqoIb6DHl7reSTxTrN8ZAYtbERdNSmccpS5Th2E3zeulYZi6GyxzY
OCMqmgliEa/2vn3I/22ZLoTcPNryFH24kPWPu6k5oeO9Oh5P5JaEF+jbESfjnruh
BWsyJIeZkx9s/oAfeBnHGheTDXDbhDOWU8EexaJRAaOUmJy7sGOjNs+fLB/OkLIE
yKTNyRyM3bA9rl6DnTTc9egc7yHpU4PBrq6LkfcUcPMth7emUC9BjyfBNh/6Z4Kf
KwP31R7m1J9arub+dAck5tPm2w==
-----END CERTIFICATE-----
```

---

### Example Attribute Mapping in Entra ID

| Logical Field | Source | Default SAML Attribute/Claim |
|---------------|---------------|------------------|
| First Name | `user.givenname` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |
| Last Name | `user.surname` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |
| Email Address | `user.mail` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |

- Email must be unique per user.  
- Custom attribute names must be communicated during setup.
---

### Assign Users

- Navigate to **Users and groups**
- Assign test and production users
- Users must be assigned before accessing the application

---

For additional assistance, please contact Focus2 support.

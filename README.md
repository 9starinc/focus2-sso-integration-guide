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
- Encrypted SAML Assertions  
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
| Encrypt Assertion | Required |

---

### 3. Required SAML Attributes

| Logical Field | Expected Attribute |
|---------------|-------------------|
| First Name | Given Name |
| Last Name | Surname |
| Email Address | Email |

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
- IdP encryption certificate (if separate)  
- Single Sign-On endpoint URL  
- SSO binding  
- Single Logout endpoint (if supported)

### Required Confirmation

Please confirm:

- SAML response is signed  
- Assertion is encrypted  
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

### Create Enterprise Application

1. Log into https://entra.microsoft.com  
2. Navigate to Enterprise Applications  
3. Create new Non-gallery application named `Focus2`

### Basic SAML Configuration

| Field | Value |
|-------|-------|
| Identifier | `https://fc.proxy.elasticsso.com/saml/sp` |
| Reply URL | `https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/saml2-acs.php/fc-sp-proxy` |

### Attribute Mapping

| SAML Claim Name | Entra Source |
|-----------------|--------------|
| `givenname` | user.givenname |
| `surname` | user.surname |
| `emailaddress` | user.mail |

It is acceptable to use the **default Microsoft Entra claim names**. No OID customization is required.

### Assign Users

Assign test and production users before access.

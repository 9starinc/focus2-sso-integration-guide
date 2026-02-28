# Focus2 Application  
# SAML 2.0 / Single Sign-On (SSO) Integration Guide

This guide provides technical instructions for integrating your organization's Identity Provider (IdP) with the **Focus2 application** using **SAML 2.0 Single Sign-On (SSO)**.

This document includes:

1. A **generic SAML 2.0 integration guide** applicable to any compliant Identity Provider  
2. A dedicated section with **Microsoft Entra ID (Azure AD) configuration instructions** as a reference implementation  

---

# Table of Contents

1. [Overview](#overview)  
2. [Focus2 Service Provider Metadata](#focus2-service-provider-metadata)  
3. [Generic SAML 2.0 Configuration Requirements](#generic-saml-20-configuration-requirements)  
4. [Required SAML Attributes](#required-saml-attributes)  
5. [NameID Requirements](#nameid-requirements)  
6. [Information Required From Customer Identity Provider](#information-required-from-customer-identity-provider)  
7. [Testing the Integration](#testing-the-integration)  
8. [Configuration Summary](#configuration-summary)  
9. [Troubleshooting Checklist](#troubleshooting-checklist)  
10. [Microsoft Entra ID (Azure AD) Configuration Example](#microsoft-entra-id-azure-ad-configuration-example)

---

# Overview

The Focus2 application supports authentication using **SAML 2.0 Single Sign-On (SSO)**.

In this integration:

- **Focus2** acts as the **SAML 2.0 Service Provider (SP)**
- Your organization’s system acts as the **Identity Provider (IdP)**

The integration requires:

- SAML 2.0 Browser SSO profile  
- Signed SAML responses  
- Encrypted SAML assertions  
- Required user attributes (First Name, Last Name, Email)

---

# Focus2 Service Provider Metadata

Focus2 provides its Service Provider metadata at:

https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/metadata.php/fc-sp-proxy

This metadata includes:

- Service Provider EntityID  
- Assertion Consumer Service (ACS) URL  
- Signing certificate  
- Encryption certificate  
- Single Logout endpoint (if configured)  

Your Identity Provider should consume this metadata when establishing the trust relationship.

---

# Generic SAML 2.0 Configuration Requirements

When configuring your Identity Provider for Focus2 SSO, ensure the following settings are applied.

## Protocol Profile

- Use **SAML 2.0 Browser SSO**

## Signing Requirements

- The **SAML Response must be signed**
- The IdP signing certificate must be included in your metadata

## Encryption Requirements

- The **SAML Assertion must be encrypted**
- Use the encryption certificate provided in the Focus2 SP metadata

## Binding

- HTTP-POST binding is recommended for SAML responses

---

# Required SAML Attributes

Focus2 requires the following attributes to provision and identify users:

- First Name  
- Last Name  
- Email Address  

Focus2 accepts valid SAML 2.0 attribute names and formats.

Please provide:

- The exact SAML2 attribute names used

Additional attributes may be requested depending on customer requirements.

---

# NameID Requirements

The NameID:

- May be configured as **Transient**
- Is not required to be a specific format
- Email is typically used as the primary identifier within Focus2

If the email address is not globally unique in your organization, configure a unique scoped identifier instead.

---

# Information Required From Customer Identity Provider

To establish trust with your Identity Provider, Focus2 requires **one of the following**:

## Option 1 – Identity Provider Metadata (Preferred)

Provide either:

- Identity Provider metadata file  
**or**
- Identity Provider metadata URL  

## Option 2 – Equivalent Configuration Details

If metadata cannot be provided, please supply the following:

- IdP EntityID  
- Signing certificate  
- Encryption certificate (if separate from signing certificate)  
- Single Sign-On endpoint URL  
- SSO binding  
- Single Logout endpoint (if supported)

## SAML 2.0 Attribute Name Requirements

Focus2 expects SAML 2.0 attributes to be included in the assertion using OID-based attribute naming.

The following attributes are required:

| Logical Field | SAML Attribute Name |
|---------------|-----------------------------------|
| First Name | `urn:oid:2.5.4.42` |
| Last Name | `urn:oid:2.5.4.4` |
| Email Address | `urn:oid:0.9.2342.19200300.100.1.3` |

### Notes

- Attribute names are case-sensitive.
- If your Identity Provider uses different attribute names, please provide the exact names being sent.
- Custom attribute names are supported, provided they are communicated during configuration.
- The email attribute must contain a unique value per user.

---

# Testing the Integration

Once configuration is complete and Focus2 confirms readiness, initiate login using:

```
https://fc.proxy.elasticsso.com/saml/module.php/elasticsso/sso.php?entityID=customer_idp_entityID
```

Replace:

```
customer_idp_entityID
```

With your Identity Provider’s EntityID.

Focus2 support will notify you when testing can begin.

---

# Configuration Summary

| Setting | Required Value |
|----------|----------------|
| Protocol | SAML 2.0 |
| Profile | Browser SSO |
| Sign Response | Yes |
| Encrypt Assertion | Yes |
| Required Attributes | First Name, Last Name, Email |
| NameID | Transient or unique identifier |

---

# Troubleshooting Checklist

If authentication fails:

- Verify EntityID matches exactly  
- Confirm SAML response is signed  
- Confirm assertion is encrypted  
- Verify required attributes are included  
- Confirm user exists and is authorized  
- Check certificate validity and expiration  
- Confirm ACS URL matches metadata  

---

# Microsoft Entra ID (Azure AD) Configuration Example

The following section provides step-by-step instructions for integrating Focus2 SSO using Microsoft Entra ID.

## Create the Enterprise Application

1. Log into https://entra.microsoft.com  
2. Navigate to **Identity → Applications → Enterprise applications**  
3. Select **+ New application**  
4. Choose **+ Create your own application**  
5. Name the application: `Focus2`  
6. Select **Integrate any other application you don't find in the gallery (Non-gallery)**  
7. Click **Create**

---

## Configure SAML Single Sign-On

1. Open the application  
2. Select **Single sign-on**  
3. Choose **SAML**

### Basic SAML Configuration

Configure the following:

| Field | Value |
|-------|-------|
| Identifier (Entity ID) | `https://fc.proxy.elasticsso.com/saml/sp` |
| Reply URL (ACS URL) | `https://fc.proxy.elasticsso.com/saml/module.php/saml/sp/saml2-acs.php/fc-sp-proxy` |
| Sign-on URL | *Leave blank* |
| Relay State | *Leave blank* |
| Logout URL | *If provided in metadata* |

Save changes.

---

## Signing Configuration

- Set signing option to: **Sign SAML response**
- Download Federation Metadata XML
- Provide this to Focus2 support

---

## Encryption Configuration

- Upload encryption certificate from SP metadata
- Enable **Encrypt assertions**
- Save changes

---

## Attribute Mapping

Navigate to **Attributes & Claims** and configure:

| SAML Attribute | Entra Source |
|---------------|--------------|
| firstName | user.givenname |
| lastName | user.surname |
| email | user.mail |

---

## NameID Configuration

Recommended:

- Source attribute: `user.mail`
- Format: `Transient` or `EmailAddress`

If email is not unique, use `user.userprincipalname`.

---

## Assign Users

- Navigate to **Users and groups**
- Assign test and production users

Users must be assigned before accessing the application.

---

For additional assistance, please contact Focus2 support.

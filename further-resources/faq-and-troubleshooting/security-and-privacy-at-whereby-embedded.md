---
description: >-
  Learn how Whereby handles privacy and security. This document is applicable
  for Whereby Embedded.
---

# Security and Privacy at Whereby Embedded

## Overview

Whereby Embedded is built by a fully remote team across Europe and beyond, with roots in Norway. As an EU-based company, we design our embedded video infrastructure with **privacy and security** as core principles.

Customers using Whereby Embedded trust us with sensitive communications data and rely on us to be a responsible custodian of their users’ information. We take this responsibility seriously and continuously evolve our security posture to meet the expectations of regulated industries such as healthcare, education, and enterprise SaaS.

Whereby:

* Does **not sell or mine user data**
* Does **not store audio or video content**
* Encrypts all data **in transit and at rest**
* Collects **only limited data required** to operate the service

***

## Standards, Certifications, and Regulatory Compliance

Whereby Embedded aligns with internationally recognized security and privacy standards and undergoes regular independent review.

### Certifications and compliance

Whereby currently maintains:

* **ISO/IEC 27001 certification**
* **GDPR compliance** as an EU-based data processor
* **HIPAA compliance** (Whereby acts as a Business Associate, not a Covered Entity)
* **Annual third-party penetration testing**, including remediation and retesting<br>

These certifications demonstrate that Whereby has an established information security management system (ISMS), documented controls, and ongoing risk management processes.

{% hint style="warning" %}
Currently, **SOC 2 Type II** reports are not available or planned, however, as we are ISO/IEC 27001 certified, this is sufficient for all of our enterprise Embedded customers.
{% endhint %}

{% hint style="info" %}
Customers can access supporting evidence through Whereby’s **Security Portal** upon request.
{% endhint %}

{% hint style="warning" %}
If you are a Free user or Build customer, and in some circumstances, an Enterprise customer, in order to access documentation in our portal, we will ask you to sign an NDA with us first.
{% endhint %}

***

## Privacy and Data Protection Principles

Whereby Embedded is designed around the following principles:

* **Data limitation** – process only what is required to deliver the service
* **Purpose limitation** – data is processed solely to enable video communication
* **User control** – customers control meeting creation, participation, and recordings

Whereby complies with applicable data protection laws, including GDPR, and maintains a Data Processing Agreement (DPA) governing its role as a processor.

Further details are available in:

* [Privacy Policy](https://whereby.com/information/tos/privacy-policy/)
* [Data Processing Agreement](https://whereby.com/information/dpa/)
* [Terms of Service](https://whereby.com/information/tos/api/)

***

## Data Storage and Processing

### What data is processed

Whereby Embedded processes limited personal data related to **account management and meeting setup**, including:

* Display name
* Email address
* User role (admin / non-admin)
* Organization or customer account association
* Video room metadata (e.g. room name, room ID)
* Profile or background images (if provided)

Whereby **does not process or store**:

* Audio content
* Video content
* Chat messages beyond the duration of the meeting

Media streams pass through Whereby infrastructure only to enable real-time communication.

In most of the situations **Whereby** is acting as the **controller** of personal data, notably in relation to usage data that is processed for the purposes of service provision, safety, and security; the content, including voice, video, text, and files, transmitted between different users where the data passes through the data processor’s servers; the information that is required to transmit and convey such data or optimize such transmission or conveyance, including user and device identifiers and other traffic (meta)data such as time and place of transmission.

**Whereby** acts a **processor** in the case of a customer using eg. Whereby-provided storage.

***

### Media handling (audio, video, chat)

* **Audio and video streams are never stored** by Whereby
* Chat messages exist only in the local browser during the meeting
* Whereby cannot access, replay, or retrieve meeting content
* If you need to record your sessions, please follow the following page
* If you need Whereby to record your sessions, please follow the following page<br>

#### **Small Room Size (End-to-End Encryption)**

* Media streams are encrypted using **DTLS-SRTP**
* Encryption keys are stored within AWS in **Ireland**
* Whereby cannot decrypt meeting content
* TURN servers may relay traffic if required, without breaking encryption



#### **Large Room Size**

* Media streams are encrypted in transit using **DTLS-SRTP**
* Streams are decrypted and re-encrypted temporarily for routing
* Media is **never persisted or stored**

***

## Recordings and Transcriptions

### Recordings

* **Local recordings** are stored only on the user’s device. Whereby has no access to them.
* **Cloud recordings:**
  * Can be stored in the customer’s own S3 bucket, or
  * Stored by Whereby only when the customer explicitly opts in and pays for Whereby-provided storage

{% hint style="info" %}
Whereby does not retain recordings when customer-managed storage is used.
{% endhint %}

### Live Transcriptions and Live Captions

All languages are processed through **EU-based** servers.

***

### Data storage

Whereby Embedded relies primarily on **Amazon Web Services (AWS)** to store data.

* All user account data is stored in **Ireland (EU)**

***

## Encryption and Secure Communication

* All web traffic uses **HTTPS with TLS**
* Real-time signaling uses encrypted WebSockets or HTTPS polling
* Media streams are encrypted using **DTLS-SRTP**
* Encryption is enforced regardless of room size or routing method<br>

Whereby does not support unencrypted communication modes.

***

## Security Testing and Posture Maintenance

Whereby maintains an active security program, including:

* Annual third-party penetration testing
* Ongoing vulnerability management
* Secure development lifecycle practices
* Regular internal risk assessments aligned with ISO 27001

Security considerations are integrated early in product development and reviewed as part of significant changes.

***

## Incident Response and Responsibility

Whereby maintains documented incident response procedures to:

* Detect and assess security incidents
* Contain and remediate issues
* Notify affected customers when required by law or contract

Customers are notified in accordance with GDPR, HIPAA, and contractual obligations.

***

## Shared Responsibility Model (Embedded Context)

Security in Whereby Embedded follows a **shared responsibility model**:

* **Whereby is responsible for:**
  * Infrastructure security
  * Media transport encryption
  * Platform availability and integrity<br>
* **Customers are responsible for:**
  * User authentication in their own product
  * Access control within their application
  * Lawful use of video communications
  * End-user disclosures and consent<br>

This distinction is critical for Embedded customers integrating video into their own workflows.

Customers are encouraged to enable their [Privacy Configurations](../../whereby-product-features/dashboard-preferences/privacy-configurations.md) via their global settings. Customers may also contact Whereby’s security team through established support channels, or your dedicated Customer Success Manager for detailed security or compliance inquiries.

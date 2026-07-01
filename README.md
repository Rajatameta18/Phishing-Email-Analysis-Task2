# Phishing-Email-Analysis-Task2
Elevate Labs Cyber Security Internship - Task 2: Phishing Email Analysis

## Project Overview
This project focuses on identifying indicators of compromise (IoCs) and social engineering traits in a suspicious email sample. The analysis includes evaluating the sender's origin, assessing psychological triggers, and utilizing an online header analyzer to check technical security authentication.

## Objective
To develop email threat analysis capabilities, recognize domain spoofing tactics, and analyze core technical indicators that distinguish malicious emails from authentic business communications.

## 📊 Phishing Analysis Case Study Report

### 1. Phishing Sample Analyzed
- **Subject Line:** URGENT: Your Subscription is Suspended - Update Payment Immediately!
- **Displayed Sender:** Netflix Security Team `<support-alert@netflx-billing-update.com>`
- **Target Link Provided:** `http://login.netflix.com.account-verification-portal.net/secure/update`

### 2. Identified Phishing Indicators & Traits
- **Domain Spoofing (Typosquatting):** The email purports to originate from Netflix, but the domain registration address is `netflx-billing-update.com`[cite: 2]. The deliberate spelling error (missing the letter 'i') is an attempt to deceive users inspecting the sender field cursorily[cite: 2].
- **Urgency and Coercion:** The text utilizes urgent call-to-action hooks ("instantly verify", "within 24 hours") and severe consequences ("permanent deletion") to manipulate the recipient into executing instructions without validation[cite: 2].
- **Mismatched Hyperlink Architecture:** The target URL uses a subdomain layer string `login.netflix.com` to project legitimacy. However, the root domain is actually `account-verification-portal.net`, an unverified third-party destination controlled by the external sender[cite: 2].
- **Generic Personalization:** The communication lacks genuine subscriber contextual identification details, relying on a mass-distribution placeholder ("Dear Customer").



## 🛠️ Technical Header Analysis
The raw email header was audited using the **Google Admin Toolbox MessageHeader** utility[cite: 2]. The tool parsed the routing metadata and flagged critical authentication failures[cite: 2]:

### Authentication Results Summary
| Security Protocol | Status | Details / Analysis |
| :--- | :--- | :--- |
| **SPF (Sender Policy Framework)** | 🔴 **FAIL** | The sending server IP address (`198.51.100.42`) is not authorized to send emails on behalf of the domain `netflx-billing-update.com`. |
| **DKIM (DomainKeys Identified Mail)** | 🔴 **FAIL** | Cryptographic signature verification failed, indicating the header details or email content was spoofed/altered. |
| **DMARC (Domain-based Message Authentication)** | 🔴 **FAIL** | Because both SPF and DKIM failed, the email failed strict DMARC alignment rules (`p=REJECT`), confirming it is a fraudulent sender. |

### Message Routing & Hops
The tool successfully mapped out the delivery path of the message from the originating mail host (`mail-out.netflx-billing-update.com`) directly to the receiving MX handler gateway, registering the exact delivery latencies and transport security layer protocol used (`TLSv1.3`).

## 🔒 Recommended Actions & Mitigation
1. **Immediate Deletion:** The email should be safely reported to the enterprise security team and permanently purged from the system[cite: 2].
2. **Endpoint Block:** The malicious sender domain `netflx-billing-update.com` and root link domain `account-verification-portal.net` should be blocked at the email gateway level.
3. **Local Security Controls:** Ensure development tools (like the local MySQL server found in Task 1) and network configurations remain completely isolated from public access profiles[cite: 1, 2].

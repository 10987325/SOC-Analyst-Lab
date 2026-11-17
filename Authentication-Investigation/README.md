# Windows Authentication Investigation — Wazuh SIEM

## 1. Investigation Overview

**Platform:** Windows 10  
**Endpoint:** WIN-CLIENT01  
**Account:** LocalAdmin  
**SIEM:** Wazuh  
**Environment:** Controlled Active Directory home lab  
**Investigation Date:** November 13, 2026

The objective was to investigate a failed Windows logon followed by a successful logon and understand the authentication events using Wazuh.

## 2. Events Investigated

| Event ID | Description | Findings |
|---|---|---|
| 4625 | Failed logon | LocalAdmin authentication failed |
| 4776 | Credential validation | NTLM credential validation failed |
| 4624 | Successful logon | LocalAdmin successfully logged on |

### Event 4625 — Failed Logon

- Wazuh Rule ID: 60122
- Wazuh Level: 5
- Logon Type: 2 (Interactive)
- Target account: LocalAdmin
- Status: `0xc000006d`
- SubStatus: `0xc000006a` (Incorrect password)
- Timestamp: 00:51:29.518

### Event 4776 — Credential Validation

- Target account: LocalAdmin
- Workstation: WIN-CLIENT01
- Status: `0xc000006a`
- Timestamp: 00:51:29.518
- Finding: Failed NTLM credential validation.

### Event 4624 — Successful Logon

- Wazuh Rule ID: 60118
- Wazuh Level: 3
- Target account: LocalAdmin
- Logon Type: 2 (Interactive)
- Logon ID: `0x16c292`
- Authentication Package: Negotiate
- Timestamp: 00:51:56.160

The actual authentication protocol used for the successful logon was not established.

## 3. Investigation and Correlation

The failed logon and NTLM credential-validation event occurred at approximately the same time.

A successful logon for the same account was observed approximately 27 seconds later.

The available evidence does not establish that the failed and successful logons belonged to the same logon session or used the same authentication protocol.

## 4. Assessment and Classification

**Classification: Likely Benign — Controlled Lab Activity**

The activity was intentionally generated during a controlled authentication-monitoring exercise.

The incorrect-password failure followed by successful authentication was consistent with the expected lab activity.

There was no evidence establishing brute-force activity or malicious access.

## 5. Recommended Follow-up

Monitor for:

- Event ID 4720 — User account creation.
- Event ID 4672 — Special privileges assigned.
- Event ID 4688 — Process creation.
- Event ID 4104 — PowerShell script block logging.
- Unusual authentication and network activity.

## 6. Key Learning Outcomes

- Investigated Windows authentication events in Wazuh.
- Distinguished failed and successful logons.
- Learned the meaning of Logon Type 2.
- Investigated NTLM credential validation using Event ID 4776.
- Learned that Negotiate alone does not prove Kerberos authentication.
- Practiced evidence-based SOC classification.

## 7. Lab Disclaimer

This was a controlled, benign activity performed in a personal cybersecurity home lab for educational purposes. No production systems were targeted.

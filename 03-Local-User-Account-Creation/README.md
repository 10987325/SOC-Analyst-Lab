# Windows Local User Account Creation Investigation — Wazuh SIEM

## 1. Investigation Overview

- **Platform:** Windows 10
- **Endpoint:** WIN-CLIENT01
- **SIEM:** Wazuh
- **Account Created:** SOC-LabUser03
- **Creator:** LocalAdmin
- **Environment:** Controlled Windows Active Directory home lab
- **Date:** November 18, 2026

The objective was to investigate local user account creation, account enabling and group membership changes using Windows Security Events collected by Wazuh.

## 2. Events Investigated

| Event ID | Description | Finding |
|---|---|---|
| 4720 | User account created | LocalAdmin created SOC-LabUser03 |
| 4722 | User account enabled | SOC-LabUser03 was enabled |
| 4732 | Member added to security-enabled local group | SOC-LabUser03 was added to Users |

## 3. Key Investigation Findings

### Event 4720 — User Account Creation

- Creator: LocalAdmin
- New account: SOC-LabUser03
- Computer: WIN-CLIENT01.corp.local
- Creator Logon ID: `0x578bb`
- Target SID: `S-1-5-21-4229563349-2843582059-3772689291-1002`
- Timestamp: November 18, 2026, 00:12:01.996
- Wazuh Rule ID: 60109
- Wazuh Rule Level: 8

### Event 4722 — User Account Enabled

The newly created account was subsequently enabled.

### Event 4732 — Local Group Membership Change

- Account performing the action: LocalAdmin
- Target group: Users
- Member SID: `S-1-5-21-4229563349-2843582059-3772689291-1002`
- Subject Logon ID: `0x578bb`

## 4. Event Correlation

The account creation, account enabling and group membership activity were investigated.

The available events shared Subject Logon ID `0x578bb`, supporting correlation to the same LocalAdmin logon session on WIN-CLIENT01.

## 5. Privilege Verification

The account's group membership was verified through Windows Computer Management.

The account belonged only to the local Users group.

No Administrators-group addition was observed in the events examined.

## 6. Assessment and Classification

**Classification: Benign — Controlled Lab Activity**

The account was intentionally created for this investigation.

The observed activity included account creation, enabling and standard Users-group membership.

No evidence of malicious activity or unauthorized privilege escalation was identified during this exercise.

## 7. Recommended Follow-up

Monitor for:

- Event ID 4624 — Successful logon.
- Event ID 4625 — Failed logon.
- Event ID 4688 — Process creation.
- Event ID 4104 — PowerShell script block logging.
- Event ID 4672 — Special privileges assigned.
- Event ID 4698 — Scheduled task creation.
- Event ID 7045 — New service installation.

## 8. Key Learning Outcomes

- Investigated Windows account-management events.
- Distinguished account creation from account enabling.
- Analyzed local security group membership changes.
- Correlated related events using Subject Logon ID.
- Verified account privileges through Windows GUI.
- Practiced evidence-based SOC investigation and classification.

## 9. Lab Disclaimer

This was a controlled and authorized activity performed in a personal cybersecurity home lab for educational purposes. No production systems were targeted.

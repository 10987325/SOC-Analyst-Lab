# PowerShell Download and Execution Investigation – Wazuh SIEM

## 1. Objective

Investigate PowerShell script download and execution activity on a Windows endpoint using Wazuh SIEM.

The objective was to generate controlled security telemetry, investigate Windows events, and correlate evidence to understand PowerShell execution behavior.

## 2. Lab Environment

- SIEM: Wazuh 4.14.7
- Server: Ubuntu 24.04 LTS
- Endpoint: Windows client (WIN-CLIENT01)
- Script hosting: Python HTTP server
- Monitoring: Windows Security and PowerShell Operational logs

## 3. Practical Simulation

Performed the following controlled activities:

1. Hosted a harmless test file and PowerShell script on the Ubuntu server.
2. Downloaded both files to the Windows endpoint using PowerShell `Invoke-WebRequest`.
3. Attempted normal script execution.
4. Observed the execution-policy restriction.
5. Executed the harmless test script using a process-scoped `ExecutionPolicy Bypass`.
6. Investigated the resulting telemetry in Wazuh.

Test script:

```powershell
Write-Output "SOC-REPEAT-SCRIPT-EXECUTION"
```

Execution command:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File "C:\Users\Public\repeat.ps1"
```

## 4. Windows Security Events Investigated

### Event ID 4688 – Process Creation

Observed:
- New process: `powershell.exe`
- Command-line argument: `-ExecutionPolicy Bypass`
- Script path: `C:\Users\Public\repeat.ps1`
- Parent process: `powershell.exe`

**Investigation finding:** Windows recorded the creation of a PowerShell process with execution-policy bypass arguments.

### Event ID 4104 – PowerShell Script Block Logging

Observed script content:

```powershell
Write-Output "SOC-REPEAT-SCRIPT-EXECUTION"
```

**Investigation finding:** PowerShell Script Block Logging captured the script content.

## 5. Event Correlation

| Evidence | Observation |
|---|---|
| HTTP server log | Successful HTTP GET request |
| Event ID 4688 | PowerShell process creation |
| Event ID 4104 | PowerShell script-block content |
| Execution output | Harmless test message displayed |

The events provided complementary evidence about the download and execution activity.

## 6. SOC Analyst Assessment

**Classification: Benign – Controlled Lab Simulation**

The activity was intentionally generated in an isolated home lab.

However, in a production environment, downloading scripts to a public user directory and executing PowerShell with `ExecutionPolicy Bypass` would warrant investigation.

A SOC analyst should examine:
- User and host context
- Parent-child process relationships
- Script content and behavior
- File reputation
- Network connections
- Business justification

Execution-policy bypass alone does not establish malicious activity.

## 7. Skills Practiced

- Wazuh SIEM investigation
- Windows Event ID 4688 analysis
- PowerShell Event ID 4104 analysis
- Process command-line investigation
- Script download monitoring
- Event correlation
- Security triage and evidence-based classification

## 8. Conclusion

This exercise improved my practical understanding of PowerShell monitoring and Windows process investigation using Wazuh.

The key lesson: correlate multiple sources of telemetry before reaching a security conclusion.

**Environment:** Personal cybersecurity home lab  
**Purpose:** Defensive security monitoring and SOC Analyst training

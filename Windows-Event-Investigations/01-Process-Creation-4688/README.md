# Windows Process Creation Investigation — Event ID 4688

## 1. Overview

This project documents a hands-on Windows process creation investigation using a Windows endpoint integrated with the Wazuh SIEM platform.

The objective was to generate a controlled process creation event, verify Windows Security Event ID **4688**, and investigate the corresponding telemetry in Wazuh.

**Investigation type:** Benign SOC lab exercise  
**Platform:** Wazuh SIEM  
**Operating system:** Windows  
**Event ID:** 4688 — A new process has been created  
**Wazuh Rule ID:** 67027  
**Wazuh Rule Level:** 3

## 2. Lab Objective

- Enable Windows process creation auditing.
- Capture process execution details.
- Verify command-line logging.
- Investigate the event in Wazuh.
- Understand parent-child process relationships.
- Practice evidence-based SOC analysis.

## 3. Controlled Lab Activity

The following harmless command was executed in PowerShell:

```powershell
cmd.exe /c "echo SOC-LAB-PROCESS-TEST"
```

This command launches Command Prompt and prints a test message. It was intentionally executed as part of the lab exercise.

## 4. Windows Event Investigation

**Windows Security Event ID:** 4688

The event confirmed the creation of a new process.

| Field | Observed value |
|---|---|
| Event ID | 4688 |
| New process | `C:\Windows\System32\cmd.exe` |
| Parent process | `powershell.exe` |
| Executing account | LocalAdmin (lab account) |
| Process command line | `cmd.exe /c "echo SOC-LAB-PROCESS-TEST"` |

## 5. Wazuh SIEM Investigation

The corresponding event was located in:

**Wazuh Dashboard → Threat Hunting → Events**

Search filter:

```text
data.win.system.eventID: "4688"
```

Observed Wazuh details:

| Field | Result |
|---|---|
| Rule ID | 67027 |
| Rule Level | 3 |
| Description | A process was created. |
| Agent | WIN-CLIENT01 |
| Event ID | 4688 |

This confirmed that the Windows endpoint successfully forwarded the process creation telemetry to Wazuh and that Wazuh decoded the event and applied a matching rule.

## 6. Process Relationship Analysis

The investigation identified the following parent-child process relationship:

```text
powershell.exe
    |
    └── cmd.exe
```

The parent process was PowerShell, which launched Command Prompt to execute a harmless echo command.

Understanding process ancestry is important in SOC investigations because suspicious command-line activity must be evaluated in the context of the process that launched it.

## 7. SOC Analyst Assessment

**Classification: Benign — Controlled Lab Activity**

### Confirmed findings

- Windows generated Security Event ID 4688.
- The event recorded the creation of `cmd.exe`.
- PowerShell was identified as the parent process.
- The command line contained the controlled lab test string.
- Wazuh received and displayed the corresponding event.
- Wazuh applied Rule ID 67027 with Level 3.

### Investigation limitations

Event ID 4688 confirms process creation. By itself, it does not establish that a process performed malicious actions or that a security incident occurred.

A real investigation may require additional evidence, including:

- PowerShell Script Block Logging (Event ID 4104).
- File creation and modification activity.
- Process execution chains.
- Network connections.
- Authentication events.
- Persistence-related changes.

## 8. Key Learning Outcomes

Through this exercise, I practiced:

1. Windows process creation auditing.
2. Event ID 4688 investigation.
3. Command-line analysis.
4. Parent-child process relationship analysis.
5. Wazuh event collection and rule interpretation.
6. Evidence-based SOC classification.

## 9. Conclusion

This exercise demonstrated the successful collection and investigation of Windows process creation telemetry using Wazuh SIEM.

The activity was a controlled and benign lab test. The main learning outcome was understanding how a SOC analyst examines process execution evidence before deciding whether activity is suspicious or malicious.

---

**Environment:** Personal cybersecurity home lab  
**Focus:** Windows Security Monitoring | Wazuh SIEM | SOC Tier 1 Investigation

*This documentation is based on an actual controlled lab exercise.*

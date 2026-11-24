# SYSMON

# SYSMON-ENDPOINT-THREAT-HUNTING-PROJECT

### Windows Endpoint Monitoring & Threat Hunting with Sysmon

## Table of Contents

1. [Project Overview](#project-overview)
2. [Sysmon Core Event IDs](#sysmon-core-event-ids)
3. [Sysmon Installation & Configuration](#sysmon-installation--configuration)
4. [Event Log Filtering & Investigation](#event-log-filtering--investigation)
5. [Sysmon Threat Hunting Investigations](#sysmon-threat-hunting-investigations)
6. [Tools & Technologies](#tools--technologies)
7. [Conclusion](#conclusion)

---

## 1. Project Overview

This project documents a hands-on Windows endpoint monitoring and threat hunting lab using **Microsoft Sysmon (System Monitor)**, a Windows Sysinternals tool that provides detailed visibility into system activities.

The lab explores Sysmon installation, event configuration, Windows Event Viewer filtering, PowerShell-based log analysis, and the investigation of suspicious endpoint activities.

The investigations focus on identifying malicious USB activity, suspicious file execution, network connections, PowerShell-based payload execution, scheduled task persistence, and potential credential access.

## 2. Sysmon Core Event IDs

Sysmon generates detailed event records that help security analysts investigate suspicious activities across Windows endpoints.

Some important Event IDs explored in this project include:

- **Event ID 1 – Process Creation:** Monitors newly created processes.
- **Event ID 3 – Network Connection:** Records network connections initiated by processes.
- **Event ID 7 – Image Loaded:** Monitors DLL and image loading activity.
- **Event ID 8 – CreateRemoteThread:** Detects remote thread creation between processes.
- **Event ID 9 – RawAccessRead:** Monitors raw access to devices and storage.
- **Event ID 11 – File Created:** Records file creation and overwrite activity.
- **Event IDs 12, 13 & 14 – Registry Events:** Monitor registry modifications.
- **Event ID 15 – FileCreateStreamHash:** Records alternate data stream activity.
- **Event ID 22 – DNS Query:** Records DNS queries made by processes.

## 3. Sysmon Installation & Configuration

Sysmon was installed through an elevated Command Prompt using a custom configuration file.

**Installation command:**

```cmd
Sysmon.exe -accepteula -i ..\Configurations\swift.xml
```

The configuration file determines which events Sysmon collects and how they are processed.

Sysmon operates as a persistent Windows service and continues monitoring system activity across reboots.

## 4. Event Log Filtering & Investigation

Windows Event Viewer and PowerShell were used to filter and investigate collected event records.

**Windows Event Viewer:**
- Navigate to the relevant event log.
- Select Filter Current Log.
- Specify the Event ID required for the investigation.
- Examine event details and XML data for additional information.

**PowerShell Log Analysis:**

Used `Get-WinEvent` with XPath queries to retrieve specific event records and investigate system activities.

## 5. Sysmon Threat Hunting Investigations

The practical investigations covered multiple attack scenarios, including:

- **Investigation 1 – Malicious USB Activity:** Examined registry modifications, raw device access, and suspicious executable activity associated with a USB device.
- **Investigation 2 – Payload Execution:** Investigated a suspicious payload, its execution context, network activity, and back-connect port.
- **Investigation 3.1 – PowerShell Payload Execution:** Analysed suspicious PowerShell execution, registry modifications, and adversary communication.
- **Investigation 3.2 – Persistence & Credential Access:** Investigated an alternate data stream payload, scheduled task creation, hidden PowerShell execution, and suspicious access to `lsass.exe`.
- **Investigation 4 – Network Activity & C2 Investigation:** Examined network connection events to identify adversary infrastructure and communication details.

---

## 6. 📑 Lab Walkthrough & Documentation

Due to the extensive nature of this practical lab, including multiple threat hunting scenarios, Sysmon event analysis, registry investigation, PowerShell commands, network indicators, and evidence screenshots, the full step-by-step documentation has been placed in a dedicated file.

👉 [**Read the Full Step-by-Step Sysmon Threat Hunting Walkthrough**](./Sysmon-Lab-Walkthrough.md)

---

## 7. Tools & Technologies

- **Microsoft Sysmon:** Windows endpoint monitoring and event collection.
- **Windows Event Viewer:** Filtering and analysing Windows and Sysmon event logs.
- **PowerShell:** Event log filtering and system investigation using `Get-WinEvent` and XPath queries.
- **Windows Sysinternals:** Sysmon installation and configuration.
- **TryHackMe:** Practical investigation environment and extracted event logs.

---

## 8. Conclusion

This project provided practical experience in using Sysmon for Windows endpoint monitoring and threat hunting. Through multiple investigations, I explored how process execution, registry modifications, file activity, network connections, and PowerShell events can reveal suspicious behaviour.

 **Effective endpoint threat hunting requires more than collecting logs. It requires understanding event relationships, identifying suspicious patterns, and correlating evidence to uncover attacker activity and reconstruct the attack chain.**

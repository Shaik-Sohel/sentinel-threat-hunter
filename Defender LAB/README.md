# Azure Cloud SOC & Threat Hunting Lab: Microsoft Defender for Endpoint (MDE)

## Project Overview
This hands-on project demonstrates the deployment, configuration, and engineering validation of an enterprise-grade Endpoint Detection and Response (EDR) solution within a Microsoft Azure environment. 

The objective of this lab was to transition from a standard alert-driven posture to proactive threat hunting. I successfully onboarded a hybrid cloud infrastructure to **Microsoft Defender for Endpoint (MDE)** via **Microsoft Defender for Cloud**, simulated advanced multi-stage adversary behaviors mapping to the **MITRE ATT&CK Framework**, and engineered custom **Kusto Query Language (KQL)** hunting queries to isolate telemetry.

---

## Skills & Technologies Demonstrated
* **Cloud Infrastructure:** Microsoft Azure, Virtual Network (VNet), Resource Groups, Network Security Groups (NSGs).
* **EDR/XDR Platforms:** Microsoft Defender for Cloud, Microsoft Defender for Endpoint (MDE).
* **Operating Systems & Administration:** Windows Server/Client Administration, Linux Administration, Secure Shell (SSH) asymmetric key authentication (`ssh -i`).
* **SIEM & Analytics:** Kusto Query Language (KQL), Advanced Hunting, Log Analytics Workspaces.
* **Cybersecurity Frameworks:** MITRE ATT&CK (Persistence, Defense Evasion, Discovery).

---

## Architecture & Implementation Lifecycle

### Phase 1: Infrastructure Deployment & Secure Access
1. Deployed standard Virtual Machines within an isolated Azure Virtual Network (VNet).
2. Hardened access controls by disabling password-based authentication, enforcing asymmetric cryptographic key-pair authentication (`.pem` private keys) via SSH over TCP Port 22.
3. Verified local endpoint telemetry health directly via PowerShell to ensure the Windows Defender Advanced Threat Protection sensor service (`sense`) was actively executing in a `RUNNING` state.

### Phase 2: Advanced Multi-Stage Attack Simulation
To test the behavioral heuristics of the EDR sensor, I executed a multi-stage **Living off the Land (LotL)** simulation using administrative binaries to mimic real-world Advanced Persistent Threat (APT) behavior:

* **Stage 1: Directory Masquerading & Discovery (MITRE T1036 / T1082)**
  Created a spoofed system path (`C:\Intel\Logs`), generated a local discovery payload running system enumeration (`whoami /all`, `netstat -ano`), and dumped it into a script named `update_service.ps1`.
* **Stage 3: Persistence Mechanics (MITRE T1053.005)**
  Registered a new rogue Windows Scheduled Task (`SystemTelemetryUpdate`) running under the elevated `NT AUTHORITY\SYSTEM` context to run the script at startup.
* **Stage 4: AMSI Evasion (MITRE T1562.001)**
  Executed a memory-patching sequence to disable the Antimalware Scan Interface (AMSI) structure (`amsiInitFailed`), simulating a defense evasion bypass attempt.

---

## Threat Hunting & Detection Engineering (KQL)

Rather than relying on out-of-the-box static alerts, I utilized **Advanced Hunting** to parse raw `DeviceProcessEvents` and `DeviceFileEvents` to build custom detection logic.

### Hunting Query 1: Identifying Rogue Scheduled Task Persistence
This query detects attempts to establish persistence by monitoring the `schtasks.exe` process or PowerShell binaries initiating task configurations targeting non-standard system directories.

```kusto
DeviceProcessEvents
| where Timestamp > ago(2h)
| where InitiatingProcessFileName =~ "schtasks.exe" or ProcessCommandLine has "Register-ScheduledTask"
| project TimeGenerated, DeviceName, InitiatingProcessAccountName, ProcessCommandLine
| top 10 by TimeGenerated desc

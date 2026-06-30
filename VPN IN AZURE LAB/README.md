# Azure SIEM Lab: Detecting Impossible Travel Anomalies in Microsoft Sentinel

## Project Overview
This project demonstrates how to simulate and detect a credential theft attack—specifically an **"Impossible Travel"** anomaly—using Microsoft Sentinel SIEM. The objective of this phase is to manipulate routing paths via a VPN to force Microsoft Entra ID to log authentication requests from two geographically distinct locations within an impossible timeframe.

---

## Architecture Flow



1. **Baseline Login:** Connection via local ISP (Origin: India).
2. **Routing Shift:** Directing all host traffic through an encrypted VPN tunnel to a remote exit node.
3. **Anomalous Login:** Connection via Private Browser session (Origin: Foreign VPN IP).
4. **Log Ingestion:** Entra ID `SigninLogs` ingested into Log Analytics for Sentinel analysis.

---

## Phase 1: Attack Simulation Steps

### 1. Establishing the Baseline (Location A)
* Ensured the desktop VPN client was completely **Disconnected**.
* Opened a standard web browser and successfully authenticated to the **Azure Portal** (`portal.azure.com`).
* This action forced the authentication system to record a baseline event originating from my actual geographical location.

### 2. Simulating the Teleportation (Location B)
* Launched the **ProtonVPN** desktop application on the physical host machine.
* Initiated a secure connection to a remote exit node location (e.g., United States / Netherlands).
* Opened a **Private/Incognito Browser window** to bypass any cached session cookies and force a clean authentication handshake.
* Authenticated to the exact same Azure user account.

---

## Evidence & Verification

### Public IP Verification
Before verifying inside the SIEM workspace, a public IP lookup confirmed that the host routing architecture had successfully shifted to the foreign endpoint:

### Sentinel Ingestion Verification
After a 10-minute ingestion buffer, the following KQL script was executed in the Microsoft Sentinel **Logs** console to verify log arrival:

```kql
SigninLogs
| where TimeGenerated > ago(1h)
| where Status.errorCode == 0
| project TimeGenerated, UserPrincipalName, Location, IPAddress, AppDisplayName

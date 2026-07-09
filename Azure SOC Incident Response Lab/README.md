# Azure SOC Incident Response Lab: RDP Brute Force Detection & Containment

## Overview

This project simulates a real-world SOC incident where an attacker performs an RDP brute force attack against an Azure Windows Virtual Machine.

The attack was detected using Microsoft Sentinel and investigated using KQL queries. The incident response process included log analysis, threat investigation, and containment actions.

## Tools Used

- Microsoft Sentinel
- Microsoft Defender XDR
- Azure Virtual Machine
- Log Analytics Workspace
- KQL
- Windows Security Event Logs
- Kali Linux

## Attack Simulation

Attack:
RDP Brute Force Authentication Attempt

Source:
Kali Linux

Target:
Azure Windows VM

Detection:
Windows Security Event ID 4625 (Failed Login)

Response:
- Investigated source IP
- Checked successful logins
- Blocked malicious IP using Azure NSG
- Recommended account hardening

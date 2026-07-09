# Microsoft Sentinel SOAR Email Alert Automation Lab

## Project Overview

This project demonstrates the implementation of a Security Orchestration, Automation, and Response (SOAR) workflow using **Microsoft Sentinel**.

The goal of this lab was to automatically detect suspicious activity, create a Sentinel incident, trigger an automation workflow, and send an email notification to the SOC team without manual intervention.

---

# Architecture

Windows Azure VM
|
|
v
Azure Monitor Agent (AMA)
|
|
v
Data Collection Rule (DCR)
|
|
v
Log Analytics Workspace
|
|
v
Microsoft Sentinel
|
|
v
Analytics Rule
|
|
v
Incident Creation
|
|
v
Automation Rule
|
|
v
SOAR Email Notification
|
|
v
SOC Analyst

---

# Tools and Technologies Used

- Microsoft Azure
- Microsoft Sentinel
- Azure Log Analytics Workspace
- Azure Monitor Agent (AMA)
- Data Collection Rule (DCR)
- Kusto Query Language (KQL)
- Microsoft Sentinel Analytics Rules
- Microsoft Sentinel Automation Rules
- SOAR Workflow

---

# Lab Objective

The objective of this lab was to:

- Collect Windows security logs from an Azure Virtual Machine.
- Detect failed login attempts using Microsoft Sentinel.
- Automatically create security incidents.
- Configure Sentinel Automation Rules.
- Trigger an automated email notification workflow.
- Simulate a real SOC alerting process.

---

# Implementation Steps

## 1. Azure VM Log Collection Setup

Created an Azure Windows Virtual Machine and configured log collection.

Components configured:

- Azure Monitor Agent (AMA)
- Data Collection Rule (DCR)
- Log Analytics Workspace

Windows Security Events were successfully collected in Microsoft Sentinel.

---

# 2. Log Verification Using KQL

Verified incoming security events using Kusto Query Language.

Example query:

```kql
SecurityEvent
| where EventID == 4625



3. Created Microsoft Sentinel Analytics Rule

Created a scheduled analytics rule to detect multiple failed login attempts.

Detection logic:

SecurityEvent
| where EventID == 4625
| summarize FailedAttempts=count() by Account, Computer, IpAddress
| where FailedAttempts >= 5

The rule detects repeated authentication failures and generates an alert.

4. Automatic Incident Creation

Enabled incident creation from the analytics rule.

Workflow:

Security Event
      |
      v
Analytics Rule Triggered
      |
      v
Sentinel Incident Created

The incident was automatically generated inside Microsoft Sentinel.

5. Configured Sentinel Automation Rule

Created an Automation Rule to process newly generated incidents.

Automation actions:

Detect failed login incidents
Add incident tags
Modify incident severity
Assign incident owner

Example:

Incident Created
        |
        v
Automation Rule Triggered
        |
        v
Incident Updated Automatically

6. SOAR Email Notification Workflow

Configured automation to send an email notification when a Sentinel incident was created.

Email included:

Incident name
Severity
Detection source
User information
Host information
Source IP address
Incident URL
Recommended investigation steps

Workflow:

Sentinel Incident
        |
        v
Automation Workflow
        |
        v
Email Notification
        |
        v
SOC Analyst
Testing and Validation
Simulation

Generated multiple failed login attempts on the Azure VM.

Example:

Username:
Administrator

Incorrect Password Attempts:
5+
Detection Result



Conclusion

This project demonstrates an end-to-end SOC workflow using Microsoft Sentinel, from log collection and threat detection to automated incident response using SOAR capabilities.

The implementation represents a real-world Security Operations Center process where alerts are automatically detected, enriched, and delivered to analysts for investigation.


This README is suitable for a **SOC Analyst portfolio/GitHub project** and matches the workflow you actually built (AMA → DCR → Sentinel → Analytics Rule → Automation Rule → Email SOAR).

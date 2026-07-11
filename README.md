# Microsoft Sentinel SOC Monitoring & Automated Incident Response Lab

![Microsoft Sentinel](https://img.shields.io/badge/Microsoft-Sentinel-blue)
![Azure](https://img.shields.io/badge/Azure-Cloud-blue)
![SOAR](https://img.shields.io/badge/SOAR-Automation-green)
![KQL](https://img.shields.io/badge/KQL-Threat%20Hunting-orange)

## Project Overview

This project demonstrates the design and implementation of a complete **Security Operations Center (SOC) environment using Microsoft Sentinel**.

The objective of this lab was to simulate a real-world enterprise SOC workflow:

- Collect security telemetry from Azure resources and endpoints
- Monitor authentication and identity activities
- Create custom detection rules using KQL
- Perform threat hunting
- Automate incident response using Logic Apps (SOAR)
- Generate automated email notifications
- Integrate ticketing systems
- Monitor Microsoft Defender and Entra ID security events

The project covers the complete SOC lifecycle:

**Detection → Investigation → Automation → Response → Ticket Creation → Notification**

---

# Architecture Overview
                Internet
                   |
                   |
             Azure Environment
                   |
    --------------------------------
    |                              |
Azure VM                    Entra ID
    |                              |

    Windows Security Logs Identity Logs
| |
--------------------------------
|
Log Analytics Workspace
|
Data Collection Rules
|
Microsoft Sentinel
|
---------------------------------
| | |
Analytics Rules Workbooks Hunting Queries
|
Sentinel Incidents
|
Logic Apps SOAR
|

| | |
Email Alert Jira Ticket Automation


---

# Technologies Used

## Cloud & SIEM

- Microsoft Azure
- Microsoft Sentinel
- Log Analytics Workspace
- Azure Virtual Machines

## Security Monitoring

- Microsoft Defender for Cloud
- Microsoft Defender for Endpoint
- Microsoft Entra ID Monitoring
- Windows Security Events

## Detection Engineering

- Kusto Query Language (KQL)
- Scheduled Analytics Rules
- Custom Detection Rules
- Threat Hunting Queries

## SOAR Automation

- Azure Logic Apps
- Automated Email Notification
- Incident Enrichment
- Ticket Creation

## Ticket Management

- Jira Integration
- Automated Incident Ticket Creation

---

# Project Implementation

---

# 1. Azure Infrastructure Setup

## Created Azure Resources

### Resource Group

Created dedicated resource group for SOC monitoring resources.

Resources:

- Virtual Machine
- Virtual Network
- Network Security Group
- Log Analytics Workspace
- Microsoft Sentinel Instance


---

# 2. Azure Virtual Machine Deployment

Created Windows Virtual Machine to generate security telemetry.

Configuration:

- Windows Server OS
- Enabled Security Monitoring
- Configured Network Security Group
- Installed required agents

Purpose:

- Generate authentication logs
- Simulate failed login attempts
- Monitor endpoint activities

---

# 3. Log Analytics Workspace Configuration

Created Log Analytics Workspace as the central repository for security logs.

Collected:

- Windows Security Events
- Authentication Logs
- Azure Activity Logs
- Defender Alerts
- Identity Logs


Data Flow:
VM Logs
|
Azure Monitor Agent
|
Data Collection Rule
|
Log Analytics Workspace
|
Microsoft Sentinel


---

# 4. Data Collection Rules (DCR)

Configured Data Collection Rules to control:

- What data is collected
- Which machines send logs
- Destination workspace


Configured:

- Windows Security Events
- Event ID Monitoring
- Azure Monitor Agent Collection


Example Events:
4625 - Failed Login Attempt

4624 - Successful Login

4720 - User Account Created

4728 - User Added To Security Group

4688 - Process Creation


---

# 5. Microsoft Sentinel Deployment

Enabled Microsoft Sentinel on Log Analytics Workspace.

Configured:

- Data connectors
- Analytics rules
- Incident management
- Investigation tools
- Workbooks


Sentinel Components:
Data Connectors
|
Analytics Rules
|
Incidents
|
Investigation
|
Automation


---

# 6. Data Connectors Configuration

Configured multiple Sentinel data connectors.

## Connected Sources:

### Windows Security Events

Collected:

- Login activity
- Privilege changes
- Account changes


### Azure Activity Logs

Monitoring:

- Resource changes
- Administrative actions


### Microsoft Entra ID

Monitoring:

- User authentication
- Risky sign-ins
- Impossible travel
- MFA failures


### Microsoft Defender

Integrated:

- Defender alerts
- Endpoint detections
- Security incidents

---

# 7. Entra ID Identity Monitoring

Implemented identity threat monitoring.

Detected scenarios:

## Impossible Travel Detection

Example:

User login:
India
|
|
USA


within a short time period.

Detection:

- Unusual location
- Abnormal sign-in pattern
- Risky authentication


## Monitoring:

- Failed sign-ins
- MFA failures
- New user creation
- Privileged account activities


---

# 8. Custom Detection Rules Using KQL

Created custom Sentinel analytics rules.

---

Brute Force Detection
SecurityEvent
| where EventID == 4625
| summarize Attempts=count()
by IpAddress, Account
| where Attempts > 10

---

9. Automated Analytics Rules

Configured Sentinel Analytics Rules:

Rule Types:
Scheduled Query Rules
Microsoft Security Rules
Fusion Detection Rules

Configured:

Severity
MITRE ATT&CK Mapping
Entity Mapping
Incident Creation

Example:

Failed Login Detection

        |
        |
 Sentinel Alert

        |
        |
 Incident Created

 ---
10. Threat Hunting Activities

Performed proactive threat hunting using KQL.

Hunting areas:

Authentication Investigation
Failed login attempts
Privileged login activity
Endpoint Investigation
Suspicious processes
Malware indicators
Identity Investigation
Risky users
Impossible travel

Example:

SecurityEvent
| where EventID == 4624
| summarize count()
by Account

---
11. Microsoft Defender Lab

Integrated Microsoft Defender security solutions.

Monitored:

Endpoint alerts
Malware detections
Vulnerability findings
Security recommendations

Workflow:

Defender Alert

      |

Microsoft Sentinel

      |

Incident Investigation

      |

Automated Response

---

12. SOAR Automation Using Logic Apps

Created automated incident response workflows.

Automation Tasks:

Trigger when Sentinel incident is created
Extract incident details
Send email notification
Create Jira ticket automatically

Workflow:

Sentinel Incident Created

          |

Logic App Trigger

          |

Collect Incident Details

          |

Send Email

          |

Create Jira Ticket

---

13. Automated Email Notification

Configured Logic App email automation.

When a high severity incident occurs:

SOC Analyst receives:

Alert Name
Severity
Time Generated
User Information
Investigation Link

Example:

Subject:
Critical Security Alert Detected

Body:

Incident:
Multiple Failed Login Attempts

Severity:
High

Account:
Administrator

Action Required:
Investigate Immediately

---

14. Jira Ticket Automation

Integrated Jira with Sentinel SOAR.

Automatic ticket creation workflow:

Sentinel Incident

       |

Logic Apps

       |

Jira Connector

       |

Security Ticket Created

Ticket Contains:

Incident Title
Description
Severity
Detection Time
Investigation Details

Example:

Ticket:

SEC-101

Title:
Possible Brute Force Attack

Priority:
High

Status:
Open

---

Author

Shaik Sohel

Cybersecurity | SOC Analyst | Azure Security Enthusiast

GitHub:
https://github.com/Shaik-Sohel

This README will present your Sentinel work closer to a **real SOC L1/L2 portfolio project** rather than a basic lab. You can also add screenshots in folders like:


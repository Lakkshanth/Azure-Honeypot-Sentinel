# Azure Honeypot with Microsoft Sentinel SIEM

![Microsoft Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Microsoft Sentinel](https://img.shields.io/badge/Microsoft_Sentinel-5E5E5E?style=for-the-badge&logo=microsoft&logoColor=white)
![Kusto Query Language](https://img.shields.io/badge/Kusto_Query_Language-00599C?style=for-the-badge)
![Azure Monitor](https://img.shields.io/badge/Azure_Monitor-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)

## Overview

This project demonstrates the deployment of a cloud-based **Windows Honeypot** on Microsoft Azure integrated with **Microsoft Sentinel (SIEM)** for centralized log collection, monitoring, threat detection, and attack visualization.

The project simulates an intentionally exposed Windows virtual machine to attract unauthorized login attempts. Windows Security Event Logs are collected using Azure Monitor Agent (AMA), forwarded to Azure Log Analytics Workspace, analyzed using Kusto Query Language (KQL), enriched with GeoIP intelligence, and monitored through Microsoft Sentinel.

---

## Project Resources

📄 **Project Report:** `docs/Azure_Honeypot_Report.pdf`

🎥 **Project Demonstration:** `demo/Azure_Honeypot_Demo.mp4`

---

## Table of Contents

- Overview
- Project Objectives
- Key Features
- Technologies Used
- Architecture
- Project Workflow
- KQL Queries
- Event IDs Monitored
- Skills & Technologies
- Learning Outcomes
- Future Improvements
- Repository Structure
- References
- Author

---

# Project Objectives

- Deploy a Windows virtual machine in Microsoft Azure.
- Configure the VM as an internet-facing honeypot.
- Capture failed login attempts from external sources.
- Forward Windows Security Events to Azure Log Analytics Workspace.
- Integrate Microsoft Sentinel for centralized security monitoring.
- Analyze logs using Kusto Query Language (KQL).
- Enrich security events with GeoIP intelligence.
- Monitor attack activity through Microsoft Sentinel.

---

# Key Features

- Windows 10 Honeypot hosted on Microsoft Azure
- Microsoft Sentinel SIEM integration
- Centralized log collection using Azure Log Analytics Workspace
- Windows Security Event monitoring
- Failed login detection (Event ID 4625)
- GeoIP enrichment using Sentinel Watchlists
- Threat hunting using Kusto Query Language (KQL)
- Security event analysis for unauthorized login attempts

---

# Technologies Used

- Microsoft Azure
- Azure Virtual Machine (Windows 10)
- Microsoft Sentinel
- Azure Log Analytics Workspace
- Azure Monitor Agent (AMA)
- Azure Network Security Group (NSG)
- Data Collection Rules (DCR)
- Kusto Query Language (KQL)
- GeoIP Watchlists
- Windows Event Viewer

---

# Architecture

```text
                Internet
                    │
                    ▼
        Windows 10 Azure VM
          (Honeypot Machine)
                    │
      Windows Security Events
                    │
                    ▼
      Azure Monitor Agent (AMA)
                    │
                    ▼
   Log Analytics Workspace (LAW)
                    │
                    ▼
        Microsoft Sentinel
                    │
                    ▼
     Threat Detection & Analysis
```

---

# Project Workflow

## Step 1 – Azure Environment Setup

- Created an Azure subscription.
- Logged into the Azure Portal.
- Prepared the cloud environment for deployment.

---

## Step 2 – Deploy the Windows Honeypot

Created a Windows 10 Azure Virtual Machine with:

- Public IP Address
- Administrator credentials
- Network Security Group (NSG)

Configured the VM by:

- Allowing inbound traffic through the NSG.
- Disabling Windows Firewall.
- Making the virtual machine accessible to external connections.

This configuration enables the VM to function as a basic honeypot for observing unauthorized login attempts.

---

## Step 3 – Generate Security Events

To verify logging functionality:

- Performed multiple failed login attempts.
- Logged into the virtual machine.
- Opened Windows Event Viewer.
- Verified Event ID **4625 (Failed Logon)**.

These events are later forwarded to Microsoft Sentinel for analysis.

---

## Step 4 – Configure Log Collection

Created:

- Azure Log Analytics Workspace
- Microsoft Sentinel Instance

Configured:

- Windows Security Events via Azure Monitor Agent (AMA)
- Data Collection Rule (DCR)

This allows Windows Security Events to be collected centrally for monitoring and investigation.

---

## Step 5 – Query Security Logs Using KQL

Example query used to retrieve failed login events:

```kql
SecurityEvent
| where EventID == 4625
```

This query retrieves all failed authentication attempts recorded by Windows Security Logs.

---

## Step 6 – GeoIP Log Enrichment

Imported the GeoIP Watchlist into Microsoft Sentinel.

The watchlist associates attacker IP addresses with:

- Country
- State
- City
- Latitude
- Longitude

Example KQL query:

```kql
let GeoIPDB_FULL = _GetWatchlist("geoip");

SecurityEvent
| where EventID == 4625
| evaluate ipv4_lookup(
    GeoIPDB_FULL,
    IpAddress,
    network
)
```

After enrichment, attacker IP addresses can be associated with geographic locations for improved analysis.

---

## Step 7 – Attack Monitoring

Microsoft Sentinel was used to monitor failed authentication attempts by analyzing:

- Attacker IP Address
- Country
- City
- Geographic Coordinates
- Failed Login Attempts
- Attack Sources

---

# KQL Queries

The repository contains the following Kusto Query Language scripts:

### `queries/failed_logins.kql`

Retrieves failed login attempts.

```kql
SecurityEvent
| where EventID == 4625
| order by TimeGenerated desc
```

---

### `queries/geoip_lookup.kql`

Enriches failed login events with geographic information.

```kql
let GeoIPDB_FULL = _GetWatchlist("geoip");

SecurityEvent
| where EventID == 4625
| evaluate ipv4_lookup(
    GeoIPDB_FULL,
    IpAddress,
    network
)
```

---

# Event IDs Monitored

| Event ID | Description |
|----------|-------------|
| 4625 | Failed Logon Attempt |
| 4624 | Successful Logon |
| 4634 | User Logoff |
| 4672 | Special Privileges Assigned |

---

# Skills & Technologies

- Cloud Security
- Microsoft Azure Administration
- Microsoft Sentinel SIEM
- Security Monitoring
- Threat Hunting
- Windows Security Event Analysis
- Kusto Query Language (KQL)
- Azure Log Analytics
- Incident Investigation
- Network Security
- GeoIP Intelligence
- Security Analytics

---

# Learning Outcomes

Through this project, I learned how to:

- Deploy a cloud-hosted Windows honeypot.
- Configure Azure networking for security monitoring.
- Collect Windows Security Events using Azure Monitor Agent.
- Forward logs to Azure Log Analytics Workspace.
- Integrate Microsoft Sentinel for centralized monitoring.
- Analyze failed authentication attempts using KQL.
- Enrich security events with GeoIP intelligence.
- Investigate attacker activity using cloud-based SIEM tools.

---

# Future Improvements

- Deploy multiple honeypot virtual machines.
- Monitor Linux-based systems in addition to Windows.
- Configure custom Sentinel Analytics Rules.
- Integrate Microsoft Defender for Endpoint.
- Automate incident response using Azure Logic Apps.
- Map detections to the MITRE ATT&CK Framework.
- Develop advanced threat-hunting dashboards.

---

# Repository Structure

```text
Azure-Honeypot-Sentinel/
│
├── README.md
│
├──Azure_Honeypot_Report.pdf
│
├── demo/
│   └── Azure_Honeypot_Demo.mp4
│
└── queries/
    ├── failed_logins.kql
    └── geoip_lookup.kql
```

---

# References

- Microsoft Azure Documentation
- Microsoft Sentinel Documentation
- Azure Monitor Documentation
- Kusto Query Language (KQL) Documentation

---


# Authors

- **Lakkshanth R**  
  B.Tech Computer Science and Engineering (Cyber Security)

- **Roohith R**  
  B.Tech Computer Science and Engineering (Cyber Security)

---

## License

This project is intended for educational and research purposes.

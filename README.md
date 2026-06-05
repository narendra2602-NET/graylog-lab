# Graylog SIEM Lab

## Project Overview

This project demonstrates the deployment and configuration of a centralized Security Information and Event Management (SIEM) platform using Graylog in a Dockerized Ubuntu environment. The objective of the lab is to collect, process, store, and analyze Windows Event Logs from endpoint systems using NXLog agents and visualize security events through Graylog dashboards.

The lab simulates a basic SOC (Security Operations Center) environment where logs from Windows systems are centralized for monitoring, investigation, and troubleshooting purposes.

---

## Lab Architecture Diagram

```text
+------------------+
| Windows 11 Host  |
| NXLog Agent      |
+---------+--------+
          |
          | GELF TCP/UDP
          |
          v
+------------------+
| Graylog Server   |
| Docker Container |
+---------+--------+
          |
          |
          v
+------------------+
| OpenSearch       |
| Log Storage      |
+------------------+
          |
          v
+------------------+
| Graylog Web UI   |
| Dashboards       |
+------------------+
```

---

## Prerequisites

### Hardware Requirements

* Minimum 8 GB RAM
* 2 CPU Cores
* 50 GB Storage

### Software Requirements

* Ubuntu Server 22.04 LTS
* Docker Engine
* Docker Compose
* Graylog
* OpenSearch
* MongoDB
* NXLog Community Edition
* Windows 11 Client

### Network Requirements

* Connectivity between Windows endpoint and Graylog server
* Required ports opened in firewall

| Port  | Service               |
| ----- | --------------------- |
| 9000  | Graylog Web Interface |
| 12201 | GELF Input            |
| 9200  | OpenSearch            |
| 27017 | MongoDB               |

---

## Docker Deployment

### Update Ubuntu

```bash
sudo apt update && sudo apt upgrade -y
```

### Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

### Verify Docker Installation

```bash
docker --version
docker ps
```

### Deploy Graylog Stack

```bash
docker compose up -d
```

### Verify Running Containers

```bash
docker ps
```

Expected Containers:

* Graylog
* OpenSearch
* MongoDB

### Access Graylog

```text
http://<server-ip>:9000
```

---

## Graylog Configuration

### Initial Login

1. Access Graylog Web UI.
2. Login using the administrator account.
3. Configure timezone and user preferences.

### Create Input

Navigate to:

```text
System → Inputs
```

Configured Inputs:

* GELF TCP
* GELF UDP

### Input Configuration

| Setting             | Value    |
| ------------------- | -------- |
| Bind Address        | 0.0.0.0  |
| Port                | 12201    |
| Receive Buffer Size | Default  |
| TLS                 | Disabled |

### Verify Message Reception

Navigate to:

```text
Search → Messages
```

Confirm that logs are being received from Windows endpoints.

---

## NXLog Configuration

### Install NXLog

Install NXLog Community Edition on the Windows endpoint.

### Configure Log Collection

Collected Event Logs:

* Security
* System
* Application
* PowerShell Operational
* Windows Defender

### Configure Output Module

Example:

```xml
<Output graylog>
Module om_udp
Host <GRAYLOG-IP>
Port 12201
OutputType GELF
</Output>
```

### Restart NXLog Service

```powershell
Restart-Service nxlog
```

### Verify Agent Status

```powershell
Get-Service nxlog
```

---

## Log Sources

The following Windows log sources were integrated into Graylog:

### Security Logs

Examples:

* Successful Logons (4624)
* Failed Logons (4625)
* Account Lockouts
* Privilege Escalation Events

### System Logs

Examples:

* Service Start/Stop Events
* Driver Events
* System Shutdowns
* Reboots

### Application Logs

Examples:

* Application Crashes
* Software Errors
* Service Failures

### PowerShell Logs

Examples:

* Script Execution
* Command History
* Administrative Activity

### Windows Defender Logs

Examples:

* Malware Detection
* Threat Remediation
* Scan Results

---

## Sample Logs

### Successful Login Event

```json
{
  "EventID": 4624,
  "Level": "Information",
  "Source": "Microsoft-Windows-Security-Auditing"
}
```

### Failed Login Event

```json
{
  "EventID": 4625,
  "Level": "Warning",
  "Source": "Microsoft-Windows-Security-Auditing"
}
```

### PowerShell Execution Event

```json
{
  "EventID": 4104,
  "Channel": "PowerShell"
}
```

---

## Dashboards

The Graylog dashboard was configured to visualize security events and system activity.

### Dashboard Widgets

* Total Messages
* Message Rate
* Top Event IDs
* Log Sources
* Failed Login Attempts
* PowerShell Activity
* Windows Defender Events

### Dashboard Benefits

* Real-time monitoring
* Security event visibility
* Centralized log analysis
* Faster incident investigation

---

## Security Use Cases

### User Authentication Monitoring

Monitor successful and failed logon attempts across endpoints.

### Privileged Activity Monitoring

Track administrative and elevated account activity.

### PowerShell Monitoring

Identify suspicious script execution and command activity.

### Malware Detection

Monitor Windows Defender events for threats and remediation actions.

### Endpoint Health Monitoring

Track system crashes, service failures, and operating system issues.

### Incident Investigation

Search historical logs for forensic analysis and troubleshooting.

---

## Troubleshooting

### Containers Not Running

```bash
docker ps -a
```

### Check Container Logs

```bash
docker logs graylog
docker logs opensearch
docker logs mongodb
```

### Restart Containers

```bash
docker compose restart
```

### Verify Input Port

```bash
sudo netstat -tulnp | grep 12201
```

### Verify NXLog Status

```powershell
Get-Service nxlog
```

### Verify Log Transmission

Generate a test event on Windows and verify it appears in Graylog search results.

---

## Lessons Learned

Through this project, the following concepts were learned:

* SIEM architecture and design
* Centralized log management
* Docker container deployment
* Graylog administration
* Windows Event Logging
* NXLog configuration
* Security monitoring workflows
* Dashboard creation and visualization
* Log search and investigation techniques
* Basic SOC operations

---

## Future Enhancements

Planned improvements for the lab include:

* Email alerting
* Event correlation rules
* Graylog pipelines
* Threat intelligence integration
* Linux log collection
* Syslog device integration
* SSL/TLS secured inputs
* Active Directory integration
* Custom security dashboards
* Automated incident response workflows

---

## Author

Narendra Tiwari

Cybersecurity • SIEM • SOC Operations • Log Management

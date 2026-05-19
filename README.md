# Phase 3 — Centralized Logging, Sysmon Telemetry, and SIEM Integration

## Project Overview

This phase focused on building centralized visibility across the homelab environment using Splunk Enterprise, Splunk Universal Forwarders, Sysmon, Windows Event Logs, and Linux Syslog forwarding.

The objective was to simulate enterprise-style endpoint monitoring and validate that telemetry from multiple systems was successfully ingested into the SIEM for future detection engineering and attack simulation phases.

---

## Lab Architecture

This SOC homelab consists of:
- Windows Server 2022 Active Directory
- Windows 11 Endpoint
- Ubuntu Linux Syslog Server
- Splunk Enterprise SIEM
- MikroTik segmented network environment
- Kali Linux attack system

The lab is segmented into Blue Team and Red Team networks for future attack simulation and detection engineering.
# Network Overview

| System              | Role                               | IP Address  |
| ------------------- | ---------------------------------- | ----------- |
| Splunk Server       | SIEM Platform                      | 10.20.20.30 |
| Windows Server 2022 | Active Directory Domain Controller | 10.10.10.12 |
| Windows 11          | Domain Joined Endpoint             | 10.10.10.x  |
| Ubuntu Server       | Linux Syslog Endpoint              | 10.20.20.10 |

---

# Technologies Used

* Splunk Enterprise
* Splunk Universal Forwarder
* Sysmon
* Windows Server 2022
* Windows 11
* Ubuntu Linux
* Active Directory
* VirtualBox
* MikroTik RouterOS
* Syslog
* Windows Event Viewer

---

# Lab Objectives

* Configure centralized logging
* Install Splunk Universal Forwarder on Windows and Linux systems
* Configure Sysmon telemetry collection
* Configure Ubuntu syslog monitoring
* Validate endpoint connectivity to Splunk server
* Confirm telemetry ingestion inside Splunk
* Build foundation for future attack detection scenarios

---

## Skills Demonstrated

- SIEM Administration
- Sysmon Configuration
- Windows Event Forwarding
- Linux Syslog Monitoring
- Active Directory Administration
- Endpoint Telemetry Analysis
- Splunk Search Processing Language (SPL)
- Network Segmentation
- Log Validation & Troubleshooting

---

# 1. Active Directory Configuration

## Domain Controller Setup

Windows Server 2022 configured as the Active Directory Domain Controller for the homelab environment. This server provides centralized authentication, domain services, and enterprise-style user management.

<img width="1462" height="822" alt="Active Directory- Server Manager overview" src="https://github.com/user-attachments/assets/4a755abf-d811-4672-acf3-cbafa379b163" />

*Figure: Active Directory Server Manager Overview*

---

## Active Directory User Creation

Created multiple Active Directory users to simulate a real enterprise environment. These accounts will later be used for authentication monitoring, brute-force testing, privilege escalation simulations, and attack detection scenarios.

<img width="1462" height="822" alt="Active Directory User creation" src="https://github.com/user-attachments/assets/9c67615e-35da-47d6-bdd1-04a8b1a22fe2" />

*Figure: Active Directory User creation*

---

# 2. Splunk Universal Forwarder Deployment

## Splunk Forwarder Service Validation

Validated that the Splunk Universal Forwarder service was successfully installed and actively running on the Windows endpoint. This service forwards endpoint telemetry to the centralized SIEM platform.

<img width="1462" height="822" alt="Verifying splunk forwarder" src="https://github.com/user-attachments/assets/89c33e45-4cc3-497e-8561-6bdcbbeeef1c" />

*Figure: Validate Splunk Universal Forwarder was insalled and running*

---

## Sysmon Service Validation

Verified that Sysmon was successfully installed and configured as a persistent Windows service for advanced endpoint telemetry collection.

<img width="1462" height="822" alt="Verifying sysmon telemetry" src="https://github.com/user-attachments/assets/6815d3b0-b206-44e4-90a4-15041f2b0ca5" />

*Figure: Validate sysmon was installed and running*

---

## Splunk Inputs Configuration

Configured Splunk inputs.conf to monitor:

* Windows Application logs
* Windows Security logs
* Windows System logs
* Sysmon Operational logs

Telemetry was forwarded into the `endpoint` index for centralized analysis.

<img width="1462" height="822" alt="Input config file splunk forwarder" src="https://github.com/user-attachments/assets/2bce926e-e71a-4e3b-a3fe-dd91a6d317c3" />

*Figure: Splunk inputs.conf file*

---

# 3. Sysmon Telemetry Validation

## Sysmon Process Creation Event (Event ID 1)

Validated Sysmon Process Creation logging. Event ID 1 captures detailed process execution telemetry including:

* Parent/child processes
* Process GUIDs
* Image paths
* User context

This telemetry is critical for detecting malware execution and suspicious process activity.

<img width="1462" height="822" alt="sysmon-eventid1-process" src="https://github.com/user-attachments/assets/a54b0483-46a8-45ae-9734-0b7dc4022600" />

*Figure: sysmon eventid1 process*

---

## Sysmon DNS Query Monitoring (Event ID 22)

Validated DNS query monitoring through Sysmon Event ID 22. This telemetry helps detect:

* Command-and-control activity
* Suspicious domain lookups
* Malware beaconing
* DNS tunneling techniques

  <img width="1462" height="822" alt="sysmon-eventid22 process" src="https://github.com/user-attachments/assets/816317ae-214a-4ff7-9b55-83f353d62776" />

*Figure: sysmon eventid22 process*

---

## Sysmon Network Connection Detection (Event ID 3)

Validated network connection telemetry using Sysmon Event ID 3. This event provides visibility into outbound network communications initiated by processes on the endpoint.

<img width="1462" height="822" alt="sysmon-eventid3 process" src="https://github.com/user-attachments/assets/5ecd45b8-f681-488c-998b-0da5551cb508" />

*Figure: sysmon eventid3 process*

---

# 4. Splunk SIEM Validation

## Endpoint Event Validation in Splunk

Confirmed successful ingestion of Windows endpoint telemetry into Splunk Enterprise. Logs from multiple hosts were centrally searchable and indexed.

<img width="1462" height="822" alt="splunk endpoint validation2" src="https://github.com/user-attachments/assets/0ad7acb5-75ca-4b38-99e7-79bedcc3e375" />

*Figure: splunk endpoint validation*

---

## Windows Security Event Monitoring

Validated ingestion of Windows Security logs including authentication-related events and credential activity from the Active Directory environment.

<img width="1462" height="822" alt="splunk endpoint validation" src="https://github.com/user-attachments/assets/e0ea85ce-ce0c-44a8-be93-7b386173f21c" />

*Figure: splunk endpoint validation*

---

## Endpoint Event Statistics

Used Splunk search queries to validate event volume and host visibility across the environment.

Example SPL Query:

```spl
index=endpoint
| stats count by host
```

<img width="1280" height="720" alt="splunk endpoint valdation users" src="https://github.com/user-attachments/assets/23cba9d6-91c5-4662-8f73-fac9ce15d800" />

*Figure: splunk endpoint validation users*

---

# 5. Windows Forwarding Validation

## Network Connectivity to Splunk Server

Validated persistent TCP connectivity from the Windows endpoint to the Splunk server on port 9997 using `netstat`.

This confirmed that the Universal Forwarder was successfully communicating with the SIEM platform.

<img width="1462" height="822" alt="splunk port validation" src="https://github.com/user-attachments/assets/1d1e1a08-4181-42b6-b08b-880e7a0aefdc" />

*Figure: Validate splunk server on port 9997*

---

# 6. Ubuntu Syslog Monitoring

## Linux Syslog Event Generation

Generated custom syslog events using the `logger` command and validated live syslog monitoring through `/var/log/syslog`.

Example command:

```bash
logger "Test syslog message from Ubuntu server"
```

<img width="1920" height="1080" alt="syslog validation" src="https://github.com/user-attachments/assets/a64db373-8141-49b9-9c5a-73b86feb96eb" />

*Figure: validate live syslog monitoring*

---

## Splunk Forwarder Status on Ubuntu

Validated that the Splunk Universal Forwarder was actively running on the Ubuntu Linux endpoint.

<img width="1919" height="1200" alt="splunk active in ubuntu" src="https://github.com/user-attachments/assets/064e7d5d-d725-4ce0-a2ac-d85852cf48f0" />

*Figure: Validate splunk universal forwarder on ubuntu*

---

## Ubuntu Connectivity Validation

Validated active network communication between the Ubuntu server and Splunk SIEM over port 9997.

<img width="1919" height="1200" alt="splunk port validation_ubuntu" src="https://github.com/user-attachments/assets/541730be-1432-48dd-b73e-0a3027917a0d" />

*Figure: Validate splunk server on port 9997*

---

## Linux Syslog Events Inside Splunk

Confirmed successful ingestion of Linux syslog events into Splunk Enterprise from the Ubuntu endpoint.

<img width="1280" height="720" alt="splunk syslog validation" src="https://github.com/user-attachments/assets/87302dc0-d6ce-4d00-995f-4eb17be1918c" />

*Figure: Validate successful ingestion of linux syslog events*

---

# Key Skills Demonstrated

* SIEM Deployment
* Splunk Administration
* Sysmon Configuration
* Windows Event Forwarding
* Linux Syslog Monitoring
* Active Directory Administration
* Endpoint Telemetry Collection
* Centralized Logging
* Security Monitoring
* Network Troubleshooting
* Windows & Linux Administration

---

# Outcome

Successfully built and validated a centralized enterprise-style logging environment capable of monitoring:

* Windows endpoints
* Linux systems
* Active Directory activity
* Sysmon telemetry
* Security logs
* Network activity

This phase establishes the foundation for:

* Threat detection engineering
* Attack simulations
* Incident response workflows
* SOC investigations
* Blue Team operations

---

## Lessons Learned

- Improved understanding of endpoint telemetry collection across Windows and Linux systems
- Learned how Sysmon enhances visibility into process and network activity
- Strengthened troubleshooting skills while validating Splunk Universal Forwarder connectivity
- Gained experience working with centralized logging and SIEM workflows
- Better understood how enterprise SOC environments monitor distributed endpoints
  
  ---
  
## Future Improvements

The next phase of this SOC homelab will focus on expanding detection and blue team capabilities through attack simulation and adversary emulation.

Planned enhancements include:

- Attack simulation using Kali Linux
- Adversary emulation exercises
- Detection engineering with custom SPL queries
- Splunk dashboards and alerting
- Brute-force attack detection
- PowerShell attack monitoring
- Incident response workflows
- Threat hunting exercises
- Snort IDS integration
- Blue Team analysis workflows
  
---

🔐 Learning cybersecurity through building, attacking, and defending systems.

---

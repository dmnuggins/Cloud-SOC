# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC]()

## Introduction

In this project, I built a small-scale honeynet within Azure and collected log data from various resources, funneling it into a Log Analytics workspace. I then use Microsoft Sentinel, which leverages the collected data to construct attack maps, initiate alerts, and generate incidents. Then for 24 hours, I measured some security metrics in the insecure environment, applied some security controls to harden the environment, measured metrics for another 24 hours, then showed the ensuing findings below. The metrics shown will include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram]()

## Architecture After Hardening / Security Controls
![Architecture Diagram]()

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin machine, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows]()<br>
![Linux Syslog Auth Failures]()<br>
![Windows RDP/SMB Auth Failures]()<br>
![MSSQL Auth Failures]()<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-10-14 00:51:06
Stop Time 2023-10-15 00:51:06

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11314
| Syslog                   | 1759
| SecurityAlert            | 9
| SecurityIncident         | 275
| AzureNetworkAnalytics_CL | 3160

## Attack Maps Before Hardening / Security Controls

```All map queries, except for *NSG Allowed Inbound Malicious Flows* actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```<br>
![NSG Allowed Inbound Malicious Flows]()<br>

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-10-18 03:11:18
Stop Time 2023-10-19 03:11:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9436
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

For this project, a mini honeynet was built in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment prior to applying the security controls and then they were measured again after applying the security controls to harden the environment. It was very clear that, post-hardening, the number of security events and incidents were dramatically reduced. This was a great hands-on learning experience for configuring and implementing a SOC.

*Please take note that there may be more security events and alerts generated within a 24-hour period if the  honeynet was deployed in an enterprise environment following the implementation of the security controls.*
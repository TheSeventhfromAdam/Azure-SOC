# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/G9w5dXo.png)

## Introduction

In this project, I built a mini honeynet in Azure and ingest log sources from many resources into a Log Analytics workspace, then it is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured security metrics in the insecure environment for 24 hours, applied a few security controls to harden the environment, measured metrics for another 24 hours, then showed the results below. The metrics that were shown show are the following:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/g0Vfa9l.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/DiWl7JP.png)

Architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the "BEFORE" metrics, all resources were originally deployed, fully exposed to the internet. The Virtual Machines had both of their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with their public endpoints visible to the Internet; aka, no use for Private Endpoints.

In the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all of the other resources were protected by their built-in firewalls as well as a Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/WDj5UPA.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/t2hIVAp.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/LHI0nOY.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-05-27T02:28:00.0499267Z
Stop Time 2023-05-28T02:28:00.0499267Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 110542
| Syslog                   | 3767
| SecurityAlert            | 6
| SecurityIncident         | 328
| AzureNetworkAnalytics_CL | 1034

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hour cycle, but after security controls were applied:
Start Time 2023-06-01T00:02:17.312008Z
Stop Time	2023-06-02T00:02:17.312008Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11161
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

During this project, a mini honeynet was constructed in Microsoft Azure and log sources were fed into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the provided logs. Also, metrics were measured in the insecure environment before the security controls were applied, and then after I applied security measures. One thing to make note of is that the number of security events and incidents were reduced a significant amount after the security controls were applied, which showed just how effective they were.

I have to add that if the resources within the network were utilized heavilty by regular users, it is very likely that more security events and alerts may have been generated within the 24-hour period which followed the implementation of the security controls.

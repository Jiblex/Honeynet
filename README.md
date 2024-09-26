# Implementing a Cloud SOC and Honeynet in Azure

In this project, I set up a small honeynet in Azure and ingested log sources from various resources into Azure's Log Analytics Workspace, which was then utilized by Microsoft Sentinel to create attack maps, trigger alerts, and generate incidents which were delt with in accordance to NIST 800-61. I measured some security metrics in the unprotected environment over a 24-hour period, hardened the environment by applying security controls in accordance to NIST 800-53: SC-7, and then collected the same security metrics 24 hours later to compare security performance. The results are shown below, highlighting the following metrics:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Alerts triggered in Log Analytics)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious flows detected in the honeynet)

## Infrastructure Overview

<img width="1315" alt="Screenshot 2024-09-23 at 22 58 18" src="https://github.com/user-attachments/assets/103ef8a3-cfac-4e0a-a6d6-2b04b0dd08e0">

<br>
<br>

As shown in the diagram above, the architecture consists of the following components:
- Virtual Network (VNet)
- Virtual Machines (2 Windows, 1 Linux)
- Network Security Groups (NSGs)
- Azure Key Vault
- Azure Storage Account (Blob Storage)
- Log Analytics Workspace
- Microsoft Sentinel (SIEM)

### The following diagram shows the architecture BEFORE hardening (security control):

<img width="1002" alt="Screenshot 2024-09-23 at 23 54 11" src="https://github.com/user-attachments/assets/dc345b39-f81c-4789-892f-959f893957eb">

<br>
<br>

Before hardening all resources deployed were exposed to the public Internet, with the VMs having their NSGs configured to let in all traffic (firewalls were also wide open), and all other resources had public endpoints (no private endpoints) visible to anyone on the Internet.

### The following diagram shows the architecture AFTER hardening (security control):

<img width="1132" alt="Screenshot 2024-09-24 at 00 07 13" src="https://github.com/user-attachments/assets/c144aed1-5a32-4327-875d-256f3be95aa4">

<br>
<br>

After hardening, NSGs were reconfigured to block all traffic that did not come from the admin workstation. All other resources were protected by their firewalls and their private endpoints.

## Attack Maps Before Hardening 

### inux-ssh-auth-fail
<img width="968" alt="linux-ssh-auth-fail" src="https://github.com/user-attachments/assets/a4180aac-b2ad-4547-b33b-daa8e4c3c6ac">
<br>
<br>

### mssql-auth-fail
<img width="1021" alt="mssql-auth-fail" src="https://github.com/user-attachments/assets/247cf2c2-78bf-4f1f-bf83-35ac8a44272a">
<br>
<br>

### nsg-malicious-allowed-in
<img width="1229" alt="nsg-malicious-allowed-in" src="https://github.com/user-attachments/assets/55e5ec92-4bc2-4cc5-81ee-d07992b845f0">
<br>
<br>

### windows-rdp-auth-fail
<img width="992" alt="windows-rdp-auth-fail" src="https://github.com/user-attachments/assets/fe13154b-56af-46a2-85cb-82f8ec47b5bd">
<br>
<br>


## Metrics Before Hardening

| Metric | Count |
|----------|----------|
| Security Event | 496500 |
| Syslog | 4186 |
| SecurityAlert | 1 |
| SecurityIncident | 278 |
| AzureNetworkAnalytics_CL | 4212 |

## Attack Maps After Hardening

The maps were empty as the KQL queries returned no results after hardening the environment. 

## Metrics After Hardening

| Metric | Count |
|----------|----------|
| Security Event | 18767 |
| Syslog | 0 |
| SecurityAlert | 0 |
| SecurityIncident | 0 |
| AzureNetworkAnalytics_CL | 0 |

## Incident Response

After completing the metric count before hardening, Azure Sentinel had mutiple incidents that needed handeling. I reviewed the incidents according to the NIST 800-61 computer security incident handling guide. For each incident I:

- reviewed the detection confirming that it was indeed a security incident, conducting analysis to determine the scope and impact of the incident.
- Contained the incident, eradicated the root cause by implementing appropriate security controls and recovering affected systems (if applicable).
- Conducted post-mortem analysis to identify areas of improvement.

<img width="1053" alt="Screenshot 2024-09-26 at 13 22 55" src="https://github.com/user-attachments/assets/d4075d5a-ccb1-486a-a78f-664ad597cb2f">


## Conclusion

This project involved having a small honeynet built in Azure where log sources were integrated into Azure's Log Analytics Workspace. Azure Sentinel was then used to ingest the logs and, based on custom analytics rules, trigger alerts creating security incidents. The honeynet envrionment was used to gather metrics for 24 hours, after which the environment was locked down using security controls in accordance to NIST 800-53: SC-7, showing a substantial reduction in metrics showing how effective the security controls are. 

It should be noted that the resources were not actively used during this period, had there been more users actively utilizing the resources during the 24 hour period it is likely that more events and alerts would have been generated. 


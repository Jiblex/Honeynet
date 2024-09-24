# Implementing a Cloud SOC and Honeynet in Azure

In this project, I set up a small honeynet in Azure and ingested log sources from various resources into Azure's Log Analytics Workspace, which was then utilized by Microsoft Sentinel to create attack maps, trigger alerts, and generate incidents. I measured some security metrics in the unprotected environment over a 24-hour period, hardened the environment by applying security controls, and then collected the same security metrics 24 hours later to compare security performance. The results are shown below, highlighting the following metrics:
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

## Attack Maps 

# Implementing a Cloud SOC and Honeynet in Azure

In this project, I set up a small honeynet in Azure and ingested log sources from various resources into Azure's Log Analytics workspace, which was then utilized by Microsoft Sentinel to create attack maps, trigger alerts, and generate incidents. I measured a few security metrics in the unprotected environment over a 24-hour period, hardened the environment by applying some security controls, and then collected the same security metrics 24 hours later to compare security performance. The results are shown below, highlighting the following metrics:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Alerts triggered in Log Analytics)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious flows detected in the honeynet)

## Infrastructure Overview

<img width="1315" alt="Screenshot 2024-09-23 at 22 58 18" src="https://github.com/user-attachments/assets/103ef8a3-cfac-4e0a-a6d6-2b04b0dd08e0">

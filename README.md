# Building a SOC + Honeynet in Azure (Live Traffic)
![Project Diagram](https://github.com/kangQlee/Azure-SOC/assets/49772470/4889d789-9b00-455d-9a89-4fb58b15d01b)


## Introduction

For this project, a mini honeynet was built in Azure Environment. The purpose of the honeynet was to capture logs from various sources, which were consolidated within a Log Analytics workspace for analysis. Microsoft Sentinel then utilized these data to develop attack maps, trigger alerts, and generate incidents. After the environment was set up, Microsoft Sentinel measured security metrics in an insecure environment for 24 hours to generate the "before" metrics. Following this phase, some security controls, in accordance with the NIST 800-53, were applied to harden the environment. After we secured the environment, the same metrics were measured for another 24 hours to generate "after" metrics for comparison and the results are below. 

The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in-before](https://github.com/kangQlee/Azure-SOC/assets/49772470/0f8e1b0f-1eda-47c1-9bb7-3198c97af51f)<br>
![syslog-ssh-auth-fail-before](https://github.com/kangQlee/Azure-SOC/assets/49772470/cf76c79f-f1dc-4afd-b12f-e3a0c4868f97)<br>
![windows-rdp-auth-fail-before](https://github.com/kangQlee/Azure-SOC/assets/49772470/e763b34d-cfb1-4180-9c8b-d4c8913e0cd4)<br>
![mssql-authentication-failure-before](https://github.com/kangQlee/Azure-SOC/assets/49772470/c68d51b3-3f0a-4f9a-ad30-2a9382cdf9bf)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-18 16:41:17
Stop Time 2023-08-19 16:41:17

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 6459
| Syslog                   | 13221
| SecurityAlert            | 1
| SecurityIncident         | 331
| AzureNetworkAnalytics_CL | 50

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-08-19 18:35:24
Stop Time	2023-08-20 18:35:24

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 796
| Syslog                   | 23
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

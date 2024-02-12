# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which Microsoft Sentinel then uses to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, and then showed the results below. The metrics we will show are:

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
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/kelvinforteta/honeynet-in-azure/assets/53996412/396c108a-a5b2-405f-8899-75fe6103c8c1)<br>
![Linux Syslog Auth Failures](https://github.com/kelvinforteta/honeynet-in-azure/assets/53996412/36afff81-f0aa-4cb8-b17b-02112cec6552)<br>
![Windows RDP/SMB Auth Failures](https://github.com/kelvinforteta/honeynet-in-azure/assets/53996412/2d682372-177a-4e59-98ae-358495f2558a)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-15 18:04:29
Stop Time 2023-11-16 18:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 53276
| Syslog                   | 9155
| SecurityAlert            | 12
| SecurityIncident         | 278
| AzureNetworkAnalytics_CL | 67587

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hours after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 2023-11-18 15:37
Stop Time	2023-11-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10767
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel triggered alerts and created incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied and after implementing security measures. Notably, the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

If regular users heavily utilized the resources within the network, more security events and alerts may likely have been generated within the 24 hours following the implementation of the security controls.

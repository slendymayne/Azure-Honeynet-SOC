# Azure Honeynet and SOC with Real-World Cyber Attacks
![Cloud Honeynet / SOC]([https://i.imgur.com/ZWxe03e.jpg](https://imgur.com/a/2G3H7L0))

## Introduction and Objective

In this project, a mini honeynet was constructed within the Azure platform. The objective was to capture and analyze logs from several sources, which were subsequently consolidated within a Log Analytics workspace. Microsoft Sentinel was deployed to leverage these logs by developing attack maps, creating alert triggers, and incident generation. Azure Sentinel measured the metrics of an insecure environment over a 24-hour period. Following this phase, security controls were implemented to fortify the virtual environment. Lastly, another 24-hour metric measurement phase was conducted and the results obtained from these endeavors are presented below. The metrics analyzed were:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


## Technologies, Azure Components, and Regulations Employed
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs, 1 Linux VM)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance


## Architecture BEFORE Hardening and Implementing Security Controls
![Architecture Diagram](https://imgur.com/twZXRq7)
During the "BEFORE" stage of the project, the virtual environment was deployed and exposed to the public for malicious actors to discover. The intent for this stage was to attract bad actors and observe their attack patterns. To achieve this, A Windows virtual machine hosting a SQL database was deployed and a Linux server both had their network security groups (NSGs) had Allow All congfigured. To entice these attackers even further, a storage account and key vault were deployed with public endpoints which were visible on the open internet. In this stage, the un-secured environment was monitored by Microsoft Sentinel using logs aggregated by the Log Analytics workspace.


## Architecture AFTER Hardening and Implementing Security Controls
![Architecture Diagram](https://imgur.com/OT1Ou0f)
During the "AFTER" stage of the project, the environment was hardened and security controls were implemented in order to satisfy NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:
- <b>Network Security Groups (NSGs)</b>: NSGs were hardended by blocking all inbound and outbound traffic with the exception of designated public IP address who required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls</b>: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

- <b>Private Endpoints</b>: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources were limited to the virtual network and not the public internet.


## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://imgur.com/BSsGPrd)<br>
![Linux Syslog Auth Failures](https://imgur.com/omQKBA3)<br>
![Windows RDP/SMB Auth Failures](https://imgur.com/VVS8wxd)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-06-16 23:48:28
Stop Time 2023-03-17 23:48:28

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15953
| Syslog                   | 3068
| SecurityAlert            | 52
| SecurityIncident         | 268
| AzureNetworkAnalytics_CL | 528

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-06-18 17:52:43
Stop Time	2023-08-19 17:52:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 600
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini, but effective, honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. After the implementation of stronger security controls, there was a 96% reduction in Windows Securtiy Events, 99% reduction in Linux Events, and 100% redution in security alerts, incidents and malicious inbound network traffic.

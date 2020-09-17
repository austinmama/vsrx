---

copyright:
  years: 2019
lastupdated: "2020-09-16"

keywords: reloading, os, upgrading, kvm, ha, standalone

subcollection: vsrx

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Upgrading the vSRX
{: #upgrading-the-vsrx}

There are several methods and considerations that need to be understood before upgrading your IBM Cloud Juniper vSRX. These include:

*	vSRX version level
*	Bare-metal server processor model
*	Bandwidth : 1G versus 10G
*	StandAlone or High Availability (HA)

Given these inputs, the table below lists whether or not you can use the OS reload option to upgrade your vSRX. The table also describes whether rollback is supported for the upgrade. Additional considerations include whether manual vSRX configuration migration will be required to complete the upgrade.

| Current vSRX Version  | Processor model and speed | Standalone or HA | Upgrade method  | Rollback supported |
| ------------- | ------------- | ------------- | ------------- | ------------- |	 			
| 15.1	| 1270v6 (All 1G Deployments)	| Standalone and HA	| Not Supported (Link to Not Supported Section) | N/A|
| 15.1 | All 10G Deployments | Standalone and HA | OS Reload (Link to OS Reload Upgrade Section) |	Standalone: No <BR> HA: <ul><li>Yes a Manual Rollback (No Automated Rollback action) after first server is OS Reloaded. See details below. <li>No after second server. |
| 18.4 | 1270v6 (Some 1G Deployments) |	Standalone and HA |	Not Supported (Link to Not Supported Section) |	N/A |
| 18.4 | 4210 (Some 1G Deployments) | Standalone | OS Reload (Link to OS Reload Upgrade Section) | No |
| 18.4 | 4210 (Some 1G Deployments) |	HA | OS Reload (Link to OS Reload Upgrade Section) | Yes |
| 18.4 | All 10G Deployments | Standalone |	OS Reload (Link to OS Reload Upgrade Section) | No |
| 18.4 | All 10G Deployments | HA |	OS Reload (Link to OS Reload Upgrade Section)	| Yes – When 18.4 is running with new architecture a Manual Rollback (No Automated Rollback action) after first server is OS Reloaded. See details below. <BR> <BR> No – When 18.4 is not running with new architecture. |      

Reference the table above to determine if you can upgrade your vSRX using OS reload. If you can, review the “General Upgrade Considerations” before proceeding.

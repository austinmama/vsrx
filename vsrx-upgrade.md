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

There are several methods and considerations that you need to understand before upgrading your IBM Cloud Juniper vSRX:
{: shortdesc}

*	vSRX version level
*	Bare-metal server processor model
*	Bandwidth: 1G versus 10G
*	Standalone or High Availability (HA)

Using these factors, the following table lists whether you can use the OS reload option to upgrade your vSRX. The table also describes whether rollback is supported for the upgrade. Additional considerations include whether you need a manual vSRX configuration migration to complete the upgrade.

| Current vSRX version  | Processor model and speed | Standalone or HA | Upgrade method  | Rollback supported |
| ------------- | ------------- | ------------- | ------------- | ------------- |	 			
| 15.1	| 1270v6 (All 1G deployments)	| Standalone and HA	| [Not Supported](/docs/vsrx?topic=vsrx-unsupported-upgrade) | N/A|
| 15.1 | All 10G Deployments | Standalone and HA | [OS Reload](/docs/vsrx?topic=vsrx-os-reload-upgrade) |	**Standalone:** No <BR> **HA:** <ul><li>Manual (not automated) rollbacks are allowed after the first server completes the OS reload.<li>Rollbacks are not allowed after the second server completes its OS reload. |
| 18.4 | 1270v6 (Some 1G Deployments) |	Standalone and HA |	[Not Supported](/docs/vsrx?topic=vsrx-unsupported-upgrade) |	N/A |
| 18.4 | 4210 (Some 1G Deployments) | Standalone | [OS Reload](/docs/vsrx?topic=vsrx-os-reload-upgrade) | No |
| 18.4 | 4210 (Some 1G Deployments) |	HA | [OS Reload](/docs/vsrx?topic=vsrx-os-reload-upgrade) | Yes |
| 18.4 | All 10G Deployments | Standalone |	[OS Reload](/docs/vsrx?topic=vsrx-os-reload-upgrade) | No |
| 18.4 | All 10G Deployments | HA |	[OS Reload](/docs/vsrx?topic=vsrx-os-reload-upgrade)	| **Yes** – If you are running version 18.4 with new architecture, manual (Not Automated) rollbacks are allowed after the first server completes the OS reload. <BR> <BR> **No** – If you are running version 18.4 without new architecture. |      

Reference this table to determine if you can upgrade your vSRX using OS reload. If you can, review the [General upgrade considerations](/docs/vsrx?topic=vsrx-general-upgrade) before proceeding.

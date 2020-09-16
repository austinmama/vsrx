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

# General upgrade considerations
{: #general-upgrade}

You should be aware of the following considerations when perform vSRX upgrades:

*	You may experience network disruptions when upgrading your vSRX version and should perform the upgrade during a maintenance window that supports potential network downtime. Failover will not be available until the upgrade completes, which can take several hours. For High Availability environments, vSRX configuration settings will migrate, but you should always export your settings before an upgrade.

*	For a Standalone environment, the previous configuration is not restored, so you should export and import your configuration. Refer to Importing and exporting the vSRX configuration for more information.

*	For a successful reload on an HA vSRX, the root password for the provisioned vSRX Gateway must match the root password defined in the vSRX portal, and root SSH login to the vSRX Private IP needs to be enabled.

  The password in the portal was defined when the Gateway was first provisioned, and may not match the current Gateway password. If the password was changed after the initial provisioning, then use SSH to connect to the vSRX Gateway and change the root password to match. The Readiness Check will fail if there is a password mis-match.
  {: important}

*	The vSRX configuration should not be modified during the execution of the OS Reload operations. Examples include automated software agents attempting to modify one or both vSRX nodes. Configurations changes can corrupt the OS Reload operations.
•	Before performing an OS Reload upgrade on an HA cluster, run the command `show chassis cluster status`. The nodes should be clustered with one node listed as the primary and the other node as the secondary. Ensure there are no `Monitor-failures`. If the cluster is not healthy prior to the upgrade then the upgrade may fail and may result in an extended traffic outage.

  Example of a healthy cluster:


  {: screen}

  Example of an Unhealthy cluster with Monitor-failures:

  {: screen}

*	If the IBM Cloud account has multiple vSRX Gateway instances in the same pod, make sure only one Gateway is upgraded at a time. Upgrading more than one vSRX at a time can result in IP collisions, disrupt the upgrade process, and potentially cause failures.
*	The upgrade process captures a snapshot of the current vSRX cluster configuration at the beginning of the process. Therefore, modifying the vSRX configuration during the upgrade process is strongly discouraged and may result in failures or unpredictable results. Additionally, these configuration changes will not be preserved if a rollback is initiated.
*	For HA clusters, the upgrade process requires the vSRX Chassis Cluster preemption flag for Redundancy Group 1 (RG1) to be disabled. Therefore, after the upgrade completes the flag will always be disabled but can optionally be re-enabled if desired. Run `show chassis cluster status` to view the preempt setting. 

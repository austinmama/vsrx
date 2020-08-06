---

copyright:
  years: 2019
lastupdated: "2019-11-14"

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

There are various methods and considerations for upgrading your {{site.data.keyword.vsrx_full}}.
{: shortdesc}

The process to upgrade High Availability (HA) vSRX configurations require two steps:

1. Running the `Upgrade Version` action against the vSRX gateway, which upgrades the Junos OS on the vSRX.
2. Running the `OS Reload` action against each bare-metal host, one at a time, which upgrades the Ubuntu OS.

Each of these upgrade steps requires several hours to complete. For Standalone gateways, the vSRX is out of service during the upgrade process. For HA gateways, when doing the upgrade, the vSRX fails over to another server in the cluster, and continues to process data traffic. After the upgrade is complete, the server rejoins the cluster.  

## Considerations
{: #considerations}

* The Standalone upgrade requires only an OS reload.

For a Standalone environment, the previous configuration is not restored, so you should export and import your configuration. For more information, see [Importing and exporting the vSRX configuration](/docs/vsrx?topic=vsrx-importing-exporting-vsrx-configuration).
{: important}

* The HA upgrade requires two steps: a vSRX upgrade and then the OS reload of each bare-metal host. It is recommended you confirm that the vSRX configuration is correct at each step.

* For a successful reload on an HA vSRX, the root password for the provisioned vSRX gateway must match the root password that is defined in the vSRX portal, and root SSH login to the vSRX Private IP needs to be enabled.

  The password in the portal was defined when the gateway was first provisioned, and might not match the current gateway password. If the password was changed after the initial provisioning, then use SSH to connect to the vSRX gateway and change the root password to match.
  {: note}

Performing an OS reload on both servers of the High Availability gateway at the same time destroys the vSRX cluster and cause the gateway to be out of service. If the vSRX cluster is destroyed, you must use the Rebuild Cluster option to reprovision the vSRX and re-create the HA cluster. Ensure that the OS reload of the first member is complete before requesting an OS reload of the second.
{: important}

* The vSRX configuration should not be modified during the execution of the Upgrade or OS Reload operations. Examples include automated software agents attempting to modify one or both vSRX nodes. Configurations changes can corrupt the Upgrade and OS Reload operations.

* Before performing an upgrade, run the command `show chassis cluster status`. The nodes should be clustered with one node that is listed as the primary and the other node as the secondary. Ensure that a single node is configured as primary (with higher priority) for both redundancy groups (RG), and that that node at runtime is serving as the primary for both RGs.

  If the RGs primary is not on the same node, run the command:

  ```
  request chassis cluster failover redundancy-group <RG number> node <node number>
  ```

  Then, to manually make RGs fall on the same node, run the command:

  ```
  request chassis cluster failover reset redundancy-group <RG number>`
  ```

* If the IBM Cloud account has multiple vSRX gateway instances in the same pod, make sure that only one Gateway is upgraded at a time. Upgrading more than one vSRX at a time can result in IP collisions, disrupt the upgrade process, and potentially cause failures.

* The upgrade process captures a snapshot of the current vSRX cluster configuration at the beginning of the process. Therefore, modifying the vSRX configuration during the upgrade process is discouraged and might result in failures or unpredictable results. Additionally, these configuration changes are not preserved if a rollback is initiated.

It is good practice to backup (export) your vSRX configuration settings before starting an upgrade. Details can be found [here](/docs/vsrx?topic=vsrx-importing-exporting-vsrx-configuration).
{: tip}

The upgrade process varies depending upon the vSRX gateway appliance configuration. Reference the vSRX appliance configurations to determine which upgrade steps are needed for you vSRX:

| vSRX Appliance              | Upgrade Method                                                      |
| :---:                       |                                                               :---: |
| Standalone<br>1G/10G        | Export vSRX Configuration<br>OS Reload<br>Import vSRX Configuration |
| High Availability<br>1G/10G | Export vSRX Configuration<br>vSRX Upgrade<br>OS Reload              |

For available Juniper vSRX versions and version-specific upgrade considerations, see [this topic](/docs/vsrx?topic=vsrx-ibm-cloud-juniper-vsrx-release-notes).

## Performing a vSRX upgrade (Standalone)
{: #performing-a-vsrx-upgrade-sa}

To perform a vSRX upgrade, [reload the OS](/docs/vsrx?topic=vsrx-reloading-the-os). Ensure you **change the default OS** and select the newest version from the list.

For a Standalone environment, your previous configuration is not restored, so an export/import is strongly recommended!
{: important}

## Performing a vSRX upgrade (High Availability)
{: #performing-a-vsrx-upgrade-ha}

To do a vSRX upgrade, follow these steps:

1. [Access the Gateway Appliances page](/docs/vsrx?topic=gateway-appliance-viewing-all-gateway-appliances) in the IBM Cloud console. On the device’s page, click **Upgrade Version** in the Actions drop down menu.

  ![Upgrade Version Button](images/upgrade_version_button.png)

2. On the Upgrade Version page, you can select the newest version of the OS and start the vSRX Upgrade.

  This can take 3 - 4 hours. However, there are only a few disruptions of 3 to 4 seconds each.
  {: note}

  ![Upgrade Version Page](images/upgrade_version_page.png)

3. During the upgrade process, the vSRX status shows as `Upgrading`.

  If the upgrade is successful, the gateway status changes to `Upgrade Active`. It is recommended that you now validate the vSRX configuration.

  The **Rollback Version** action is available in the menu, and can revert the vSRX to the previous version and configuration. After the OS reload process begins in step 4, the Rollback Version action is no longer be available.
  {: important}

4. Perform an OS reload on one node at a time to update the Host OS. The procedure can be found [here](/docs/vsrx?topic=vsrx-reloading-the-os). Ensure that you **change the default OS** and select the newest one.

  ![Change Default OS](images/change_default_os.png)

  The host OS password changes after the OS reload completes.
  {: note}

  For 10G (SR-IOV) upgrades only: When the first OS reload completes, that node becomes the primary node. High Availability is not enabled until the second node is OS reloaded to enable SR-IOV. Therefore, it is recommended that you run the OS reload of the second node quickly.
  {: important}

## Post-upgrade considerations
{: #post-upgrade-considerations}

* The HA upgrade process requires the vSRX Chassis Cluster preemption flag for Redundancy Group 1 (RG1) to be disabled. Therefore, after the upgrade (Step 1) is completed, the flag is always disabled. Run `show chassis cluster status` to view the `Preempt` setting. The OS Reload's (Step 2) also require this flag to be disabled. After the final OS Reload is completed, the preemption setting on RG1 can optionally be reenabled.

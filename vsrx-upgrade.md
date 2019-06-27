---

copyright:
  years: 2019
lastupdated: "2019-6-14"

keywords: reloading, os, upgrading, kvm, ha, standalone

subcollection: vsrx

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Upgrading the vSRX
{: #upgrading-the-vSRX}

The upgrade process migrates the Junos OS for vSRX software to the latest release, and usually requires several hours to complete, depending on whether it's a Standalone or High Availability (HA) gateway appliance. For Standalone Gateways, the vSRX will be out of service during the upgrade process. For HA Gateways, when doing the upgrade, the vSRX will failover to another server in the cluster, and continue to process data traffic. Once the upgrade is complete, the server will rejoin the cluster.  

## Considerations
{: #considerations}

* The Standalone upgrade requires only an OS reload.

For a Standalone environment, the previous configuration is not restored, so you should export and import your configuration. Refer to [Importing and exporting the vSRX configuration](/docs/infrastructure/vsrx?topic=vsrx-importing-exporting-vsrx-configuration) for more information.
{: important}

* The HA upgrade requires two steps: a vSRX upgrade and then the OS reload. It is strongly recommended you confirm that the vSRX configuration is correct at each step.
* When requesting OS reload, make sure to change the default OS and select the newest version.

![Change Default OS](images/change_default_os.png)

* For a successful reload on an HA vSRX, the root password for the provisioned vSRX Gateway must match the root password defined in the vSRX portal, and root SSH login needs to be enabled. 

  The password in the portal was defined when the Gateway was first provisioned, and may not match the current Gateway password. If the password was changed after the initial provisioning, then use SSH to connect to the vSRX Gateway and change the root password to match.
  {: note}

Performing an OS reload on both servers of the High Availability gateway at the same time will destroy the vSRX cluster and cause the gateway to be out of service. If the vSRX cluster is destroyed, you must use the Rebuild Cluster option to re-provision the vSRX and recreate the HA cluster. Ensure the OS reload of the first member is complete before requesting an OS reload of the second.
{: important}

* Before performing an upgrade, run the command `show chassis cluster status` to ensure that a single node is configured as primary (with higher priority) for both redundancy groups, and that that node at run time is serving as the primary for both RGs. If the RGs primary is not on the same node, run `request chassis cluster failover redundancy-group <RG number> node <node number>` and then `request chassis cluster failover reset redundancy-group <RG number>` to manually make RGs fall on the same node.

It is good practice to backup (export) your vSRX configuration settings before starting an upgrade. Details can be found [here](/docs/infrastructure/vsrx?topic=vsrx-importing-and-exporting-the-vsrx-configuration).
{: tip}

The upgrade process varies depending upon the vSRX Gateway Appliance configuration. Reference the vSRX appliance configurations to determine which upgrade steps are needed for you vSRX:

| vSRX Appliance              | Upgrade Method                                                      |
| :---:                       |                                                               :---: |
| Standalone<br>1G/10G        | Export vSRX Configuration<br>OS Reload<br>Import vSRX Configuration |
| High Availability<br>1G/10G | Export vSRX Configuration<br>vSRX Upgrade<br>OS Reload              |

For available Juniper vSRX versions and version specific upgrade considerations, please refer to [this topic](/docs/infrastructure/vsrx?topic=vsrx-ibm-cloud-juniper-vsrx-release-notes).

## Performing a vSRX upgrade (Standalone)
{: #performing-a-vsrx-upgrade-sa}

To perform a vSRX upgrade, [reload the OS](/docs/infrastructure/vsrx?topic=vsrx-reloading-the-os). Ensure you **change the default OS** and select the newest version from the list.

For a Standalone environment, your previous configuration is not restored, so an export/import is strongly recommended!
{: important}

## Performing a vSRX upgrade (High Availability)
{: #performing-a-vsrx-upgrade-ha}

To do a vSRX upgrade, perform the following procedure:

1. [Access the Gateway Appliances screen](/docs/infrastructure/vsrx?topic=vsrx-viewing-all-your-gateway-appliances) in the Customer Portal. On the device’s page, click **Upgrade Version** in the Actions drop down menu.

  ![Upgrade Version Button](images/upgrade_version_button.png)

2. On the Upgrade Version page, you can select the newest version of the OS and start the vSRX Upgrade.

  ![Upgrade Version Page](images/upgrade_version_page.png)
  
3. During the upgrade process, the vSRX status shows as "Upgrading". 

  If the upgrade is successful, the Gateway status changes to "Upgrade Active". It is recommended that you now validate the vSRX configuration. 

  The **Rollback Version** action is available in the drop down menu, and can revert the vSRX to the previous version and configuration. Once the OS reload process begins in step 5, the Rollback Version action will no longer be available.
  {: important}
  
5. Perform an OS reload on one node at a time to update the Host OS. The procedure can be found [here](/docs/infrastructure/vsrx?topic=vsrx-reloading-the-os). Ensure that you **change the default OS** and select the newest one.

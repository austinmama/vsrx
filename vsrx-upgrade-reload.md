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

# Upgrading using OS reload
{: #os-reload-upgrade}

To upgrade your vSRX using the OS reload and rollback method, perform the following procedure.

1.	Standalone environment only: Export the vSRX configuration.
2.	Access the gateway details page.
3.	Run the Readiness check for “OS reload” and address any errors that are found.
4.	Access the Hardware details page for each bare metal server.
5.	Run Actions -> OS Reload for each server, one at a time.

  When upgrading an HA cluster the process will power off the node not being OS Reloaded at the end of the upgrade process. This will transition the cluster’s primary node and any active network traffic to the newly OS reloaded node. Once the OS Reload has completed for the first node in the cluster it is critical that the second node be left powered off until the OS Reload to upgrade that node is submitted and running. Powering the node on prior to OS Reload will cause the cluster to be running with mismatched vSRX versions, potentially leading to a “split-brain” scenario where each node tries to claim primary ownership. This generally results in an outage. After the OS Reload of the first node the Gateway will transition to “Upgrade Active” status.
  {: important}

6.	Standalone environment only: Import the vSRX configuration and migrate the settings to the new architecture if necessary.

## vSRX configuration migration considerations
{: #migration-considerations}

For a High Availability environment, the previous vSRX configuration is restored as part of the upgrade process. No further steps are needed.

For a Standalone environment, the previous configuration is not restored, so you should export and import your configuration. Refer to Importing and exporting the vSRX configuration for more information. Additionally, when migrating from an older version such as 15.1, the interface mappings may have changed. This requires some modifications to the vSRX configuration post-import. Refer to Migrating 1G vSRX standalone configurations for more information.

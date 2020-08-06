---

copyright:
  years: 2018
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
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Rebuilding a High Availability vSRX cluster
{: #rebuilding-an-ha-cluster}
{: help}
{: support}

You can rebuild a High Availability (HA) {{site.data.keyword.vsrx_full}} cluster using the information here.
{: shortdesc}

Rebuilding an HA vSRX cluster is a very destructive procedure, and should be used only in a few specific scenarios. Consult your IBM Support representative before beginning this procedure.
{: important}

To rebuild one of your HA vSRX clusters, follow these steps:

1. From your browser, open [https://cloud.ibm.com ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com){:new_window} and log into your account.
2. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left, then click **Classic Infrastructure**.
3. Choose **Network > Gateway Appliances**.
4. Click the server you want to reload.
5. Click the server name in the Hardware section.
6. Click the **Actions** menu and select **Rebuild Cluster**.
7. Carefully read the warning message. The operation to rebuild a cluster is destructive. If you want to proceed, save your vSRX configuration before you click **Rebuild** to start the process.

---

copyright:
  years: 2018
lastupdated: "2019-11-14"

keywords: vsrx, patches, notes

subcollection: vsrx

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# IBM Cloud Juniper vSRX release notes
{: #ibm-cloud-juniper-vsrx-release-notes}

You can view a complete list of {{site.data.keyword.vsrx_full}} release notes here.
{: shortdesc}

For Juniper vSRX 18.4 release notes, visit [this link ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.juniper.net/documentation/en_US/vsrx/information-products/topic-collections/release-notes/18.4/index.html){:new_window}.

For Juniper vSRX 15.1 release notes, visit [this link ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.juniper.net/documentation/en_US/vsrx/information-products/topic-collections/release-notes/15.1x49/vsrx-release-notes-15.1x49-d120.pdf){:new_window}.

## Upgrade considerations
{: #Upgrading-Considerations}

* When upgrading from vSRX 10G 15.x to vSRX 10G 18, the upgrade process modifies the interface mappings in vSRX config for SR-IOV optimization, so during import the interface section must be preserved.
* Failover support within the High Availability (HA) cluster is not available during the upgrade and OS Reload steps, and is only available after the OS Reload of both nodes has completed.
* The preemption setting for all redundancy groups is disabled as a result of the vSRX 15.x to vSRX 18 upgrade. After the OS Reload of both nodes is completed, preemption can be reenabled.
* Reference [Upgrading the vSRX](/docs/vsrx?topic=vsrx-upgrading-the-vsrx) for more details.

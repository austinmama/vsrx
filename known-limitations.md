---

copyright:
  years: 2018
lastupdated: "2018-10-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Known Limitations

Current limitations for {{site.data.keyword.cloud}} IBM® Cloud Juniper vSRX:

* Juniper vSRX Gateway is deployed with networking virtualization using Linux Bridge. Linux Bridge based networking virtualization can only achieve limited throughput and never line-rate throughput.

* There is no support to upgrade from Standalone to High-availability mode.

* IBM Cloud Juniper vSRX Gateway is deployed with Junos OS version `15.1X49-D123`. Currently, there is no support for upgrading to a different version.

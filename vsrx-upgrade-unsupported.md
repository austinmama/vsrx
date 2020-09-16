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

# Unsupported upgrades
{: #unsupported-upgrade}

Early deployments of 1G vSRX 15.1 and 18.4 Gateway’s leveraged a networking design based on Linux Bridging. Newer 1G deployment’s leverage a networking design based on SR-IOV. Details on these differences can be found here.

Early 1G deployment’s generally used an Intel 1270v6 4-Core Sky-Lake based processor. This processor does not support SR-IOV. Newer vSRX version’s such as 19.4 require the SR-IOV networking design. Therefore, vSRX version upgrades are not supported for deployment’s based on this 1270v6 processor.

To upgrade to a newer vSRX version such as 19.4, a new vSRX order must be placed. Once completed, to migrate the configuration from the old design to the new design some manual configuration changes must also be applied to the new vSRX. Guidance on this process can be found here.

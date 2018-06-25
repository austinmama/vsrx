---

copyright:
  years: 2017
lastupdated: "2018-06-25"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# vSRX Basics
The vSRX can be configured using a remote console session through SSH or by logging into the web GUI. By default, the web GUI is not available from the public internet. To enable the web GUI, log in through SSH first.

**NOTE:** Configuring the vSRX outside of its shell and interface may produce unexpected results and thus it is not recommended.

## Accessing the Device Using SSH
vSRX can be accessed via ssh via public/private IP

* Go to Gateway Appliance Details screen and get the Public gateway IP or Private Gateway IP.

	![Gateway Appliance Details](images/basics.png)

* Run the command `ssh <IP>`. The username and password here will be the same as on the server.

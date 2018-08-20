---

copyright:
  years: 2017
lastupdated: "2018-07-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# vSRX Basics
The IBM Cloud Juniper vSRX gateway can be configured using a remote console session through SSH or by logging into the web GUI. By default, the web GUI is not available from the public internet. To enable the web GUI, log in through SSH first.

**NOTE:** Configuring the vSRX outside of its shell and interface may produce unexpected results and is not recommended.

## Accessing the Device Using SSH

You can access the vSRX using SSH through a public IP address, or through a private IP address if you're on SoftLayer VPN:

1. Go to Gateway Appliance Details screen and get the Public gateway IP or Private Gateway IP.

	![Gateway Appliance Details](images/basics.png)

2. Run the command `ssh customer-admin@<gateway-ip>`. The initial password will be the same as the *root* password on the server. In HA configuration, the initial password will be the same as the *root* password on the first server listed in details page.

## Accessing the Configuration Mode

Once a shell has been opened to the vSRX, you can enter the configuration mode by running `config`.  Configurations can be viewed in this mode using the `show` command.  Changes can be staged using the `set` command.  Staged changes can be viewed by running `show | compare`.  If you are happy with these changes, you will need to commit them to the active configuration by running `commit` and then `save`.  To leave Configuration mode run `exit`.


## Accessing the Device using the vSRX Web Management UI

To enable the web management GUI, run the following commands from the CLI in `configuration` mode:

```
set system services web-management https interface fxp0.0
set system services web-management https interface ge-0/0/1.0
set system services web-management https port 8443
set system services web-management https system-generated-certificate
set system services web-management session session-limit 100

set firewall filter PROTECT-IN term WEBSERVICE from destination-address <public-gateway-ip/32>
set firewall filter PROTECT-IN term WEBSERVICE from destination-address <private-gateway-ip/32>
set firewall filter PROTECT-IN term WEBSERVICE from protocol tcp
set firewall filter PROTECT-IN term WEBSERVICE from destination-port 8443
set firewall filter PROTECT-IN term WEBSERVICE then accept
```

You can now access vSRX web management at `https://gateway-ip:8443`.


## Accessing the Device using the virsh console

You can also access the vSRX from the gateway server Operating System (OS):
1. Log on your gateway server by running the command `ssh root@<server-ip>`.
2. Run thre command `virsh list` to find your vSRX VM name.
3. Run the command `virsh console <your vSRXvM name>`.

## Creating system users

By default, the IBM Cloud Juniper vSRX is configured with SSH access for the username `customer-admin`. Additional users can be added with their own set of priorities. For example:

```
set system login user ops class operator authentication encrypted-password <CYPHER>
```

In this example, `ops` is the username and `operator` is the class/permission level assigned to the user.

Customized classes can be also defined as opposed to pre-defined ones.

## Defining the vSRX hostname

You can set or change the vSRX hostname using the following command:

```
set system host-name <hostname>
```

## Configuring DNS and NTP

To configure name server resolution and NTP, run the following commands:

```
set system name-server <DNS server>
set system ntp <NTP server>
```

## Changing the root password

You can change the root password by running the following command:

```
set system root-authentication plain-text-password
```

This prompts you to input a new password, which is encrypted and stored in the configuration, and is not visible.

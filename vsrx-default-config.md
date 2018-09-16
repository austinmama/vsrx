---

copyright:
  years: 2018
lastupdated: "2018-09-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# vSRX Default Configuration
Juniper vSRX gateway devices in IBM Cloud comes with following default configuration:
* SSH and Ping are permitted on both vSRX public and private gateway IP addresses
* Juniper Web Management (J-Web) UI access is permitted on https port 8443 for both public and private gateway IP addresses
* An address-set `SERVICE` is predefined for SoftLayer service networks.
* Two security zones: `SL-PRIVATE` and `SL-PUBLIC` are predefined.
* Access from zone `SL-PRIVATE` to all services provided by SoftLayer in address-set `SERVICE` is permitted.t
* All other network accesses are denied except these described above.

### Default Configuration of a sample standalone vSRX gateway

```
(defaults/vsrx_sa_default.conf)
```

_Network interface definition in the above configuration_

| Interface Name    |  Interface Function      |
| :---          |   :---         |
| ge-0/0/0      |   Gigabit ethernet interface for SL-PRIVATE transit VLAN |
| ge-0/0/1      |   Gigabit ethernet interface for SL-PUBLIC transit VLAN  |
| fxp0          |   Management interface        |
| lo0           |   loopback interface          |



### Default Configuration of a sample vSRX gateway HA

```
(defaults/vsrx_ha_default.conf)
```

 _Network interface definition in the above configuration_

| Interface Name   |  Interface  Function      |
| :---          |    :---         |
| ge-0/0/1 / ge-0/0/2   |  Gigabit ethernet interface for Private VLAN on primary node |
| ge-0/0/3 / ge-0/0/4   |  Gigabit ethernet interface for Public VLAN on primary node |
| ge-7/0/1 / ge-7/0/2  |  Gigabit ethernet interface for Private VLAN on secondary node |
| ge-7/0/3 / ge-7/0/4  |  Gigabit ethernet interface for Public VLAN on secondary node |
| reth0         |   Redundant ethernet interface for SL-PRIVATE transit VLAN |
| reth1         |   Redundant ethernet interface for SL-PUBLIC transit VLAN  |
| reth2         |   Redundant ethernet interface for CUSTOMER Private VLANs  |
| reth3         |   Redundant ethernet interface for CUSTOMER Public VLANs   |
| fab0 / fab1   |   Chassis cluster fabric link |
| fxp0          |   Management interface        |
| lo0           |   loopback interface          |

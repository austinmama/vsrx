---

copyright:
  years: 2018
lastupdated: "2019-08-07"

keywords: ubuntu, Hypervisor

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

# Working with Ubuntu Hypervisor

{{site.data.keyword.vsrx_full}} runs as a Virtual Machine on Linux Kernel-based Virtual Machine (KVM), and the host operating system for KVM is Ubuntu. In a Highly Available configuration, each bare-metal server runs an instance of Ubuntu. As a result, changes to the host operating system should be made with caution, as configuration changes to the Ubuntu networking or firewall can affect the performance and reliability of the vSRX.

## Firewall Considerations

By default, the Ubuntu Firewall (UFW) is not enabled. If you do enable the UFW, then several considerations should be noted:

  * For HA configurations, the vSRX cluster's control plane traffic communicates over the host's `br0` interface.
  * The cluster's hosts exchange heartbeats through a GRE tunnel.
  * Enablement of the firewall will block the GRE and heartbeat communication, unless you enable a special configuration for the UFW. A symptom of this problem can be seen by running the following from the vSRX:

  ```
  show chassis cluster status
  ```

 If the output shows the node status as lost, then it is possible the firewall is blocking GRE and cluster heartbeat traffic.
    
  ```
  root@asloma-vsrx-18-10g-dual-wdc07-ha0-vSRX-Node0> show chassis cluster status    
  
  Monitor Failure codes:
  
    CS  Cold Sync monitoring        FL  Fabric Connection monitoring
  
    GR  GRES monitoring             HW  Hardware monitoring
  
    IF  Interface monitoring        IP  IP monitoring
  
    LB  Loopback monitoring         MB  Mbuf monitoring
  
    NH  Nexthop monitoring          NP  NPC monitoring              
  
    SP  SPU monitoring              SM  Schedule monitoring
  
    CF  Config Sync monitoring      RE  Relinquish monitoring
  

    Cluster ID: 7

    Node   Priority Status               Preempt Manual   Monitor-failures


    Redundancy group: 0 , Failover count: 1

    node0  100      primary              no      no       None           

    node1  0        lost                 n/a     n/a      n/a            


    Redundancy group: 1 , Failover count: 3

    node0  100      primary              yes     no       None           

    node1  0        lost                 n/a     n/a      n/a  

  ```

You can allow GRE traffic through the firewall by performing the following procedure:

1. Modify the `/etc/ufw/before.rules` to accept Protocol 47 traffic. To do so, add the following to the top of the `before.rules` file:

  ```
  -A ufw-before-input -p 47 -j ACCEPT
  ```

2. Next, run the command `ufw reload`.

  GRE traffic will now pass through the firewall.

  If the firewall is reset or the host is reloaded, this rule will not persist. To reenable it, perform this procedure again.
  {: note}

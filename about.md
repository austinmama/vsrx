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

# About
The IBM Cloud Juniper vSRX Gateway (vSRX) allows you to selectively route private and public network traffic through a full-featured enterprise router with firewall, traffic shaping, policy-based routing, VPN and a host of other features. The vSRX provides performance, ease of configuration, and the maintenance advantages of running on a normal hardware server. The hardware is sized to handle the routing load for multiple VLANs, and can be ordered with redundant network links and redundant RAID arrays. All vSRX features are customer-managed. 

The IBM vSRX provides the latest Vyatta 5600 operating system for x86 bare metal servers. It is offered as both a High Availability (HA) or standalone offering.

##Firewall
To protect your environment from external threats, the vSRX can be leveraged as a firewall. You can add firewall rules to allow or deny inbound or outbound network traffic to the ports your application runs on, and you can filter the traffic within your own networks. The vSRX can also be configured to perform stateful IPv4 and IPv6 filtering to protect your critical data.

## Virtual Private Network (VPN) Gateway
Connect your on-site data center or office to the IBM Cloud using VPN tunneling by provisioning your vSRXas a network gateway device. You can use an IPsec site-to-site VPN tunnel for secure communication from your enterprise data center or office to your IBM Cloud network. Other VPN options are; Remote access IPsec VPN (client-to-site), OpenVPN, GRE, L2TP and DMVPN.

Have a look at the Brocade VPN Configurations guides in our Supplemental vSRX documentation section

## Network Address Translation (NAT)
With the Virtual Router Appliance, you can provision application and database servers without public network interfaces while still allowing your servers to access the Internet using Source NAT. You can also hide your servers behind the gateway device with Destination NAT for enhanced security.

## Enterprise-grade Routing
For multi-tiered applications on different isolated networks, the vSRX enables you to build connectivity between these networks with greater flexibility. You will be able to setup dynamic routing using BGP, which will allow you to announce your own public IP space on the IBM Cloud routers. BGP will also offer more flexibility for custom private network configurations when using various tunnels and direct link solutions.

Have a look at the Brocade BGP Configurations guides in our Supplemental vSRX documentation section.

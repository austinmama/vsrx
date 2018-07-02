---

copyright:
  years: 2017
lastupdated: "2018-07-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# About
The IBM Cloud Juniper vSRX Firewall allows you to selectively route private and public network traffic through a full-featured enterprise level firewalll that is powered by the JunOS software supporting features such as, full routing stack, QoS and traffic sharing, policy-based routing, VPN among many others. The vSRX provides performance, ease of configuration and the maintenance advantages  simplicity of running on a bare metal server. The hardware is sized to handle the routing/security load for multiple VLANs and can be ordered with redundant network links and redundant RAID arrays. All vSRX features are customer-managed. 

The IBM vSRX is offered in two different modes: High Availability (HA) cluster or standalone.

##Firewall
The IBM Cloud vSRX is deployed in a way to protect your environment from external threats. It was designed to be able to filter both private the public facing traffics. Customers can manage the vSRX themselves by defining policies and rules to allow or deny (among other actions) inbound or outbound network traffic, therefore protecting their applications from internal and external users. Both IPv4 and IPv6 stacks are supported in a stateful manner.Remote access IPSEC VPN is also supported.

For a detailed configuration guide on VPN, please refer to the links provided in the topic [VPN](vpn.html).

## Virtual Private Network (VPN) Gateway
Connect your on-site data center or office to the IBM Cloud using VPN tunneling by provisioning your vSRXas a network gateway device. You can use an IPsec site-to-site VPN tunnel for secure communication from your enterprise data center or office to your IBM Cloud network. Other VPN options are; Remote access IPsec VPN (client-to-site), OpenVPN, GRE, L2TP and DMVPN.

Have a look at the Brocade VPN Configurations guides in our Supplemental vSRX documentation section

## Network Address Translation (NAT)
With the Virtual Router Appliance, you can provision application and database servers without public network interfaces while still allowing your servers to access the Internet using Source NAT. You can also hide your servers behind the gateway device with Destination NAT for enhanced security.

## Enterprise-grade Routing
For multi-tiered applications on different isolated networks, the vSRX enables you to build connectivity between these networks with greater flexibility. You will be able to setup dynamic routing using BGP, which will allow you to announce your own public IP space to the IBM Cloud routers. BGP will also offer more flexibility for custom private network configurations when using a mix of tunnels and direct link solutions.

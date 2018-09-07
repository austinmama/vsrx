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

# Work with firewalls
The IBM Cloud Juniper vSRX uses the concept of security zones, where each vSRX interface is mapped to a "zone", to handle stateful firewalls.  Stateless firealls are controlled by firewall filters. 

Policies are used to allow/block traffic between these defined zones, and the rules defined here are stateful.
In the IBM Cloud, a vSRX is designed to have four different security zones:

* SL-Private (untagged)
* SL-Public (untagged)
* Customer-Private (tagged)
* Customer-Public (tagged)

## Zone Policies
To configure a stateful firewall, perform the following procedure:

1. Create security zones and assign the respective interfaces:

	```
	set security zones security-zone CUSTOMER-PRIVATE interfaces ge-0/0/1.0
	set security zones security-zone CUSTOMER-PUBLIC interfaces ge-0/0/2.0
	```

2. Define the policy and rules between two different zones.

	The following example illustrates pinging traffic from the zone `Customer-Private` to `Customer-Public`:

	```
	set security policies from-zone CUSTOMER-PRIVATE to-zone CUSTOMER-PUBLIC policy ALLOW_ICMP match source-address any destination-address any application junos-icmp

	set security policies from-zone CUSTOMER-PRIVATE to-zone CUSTOMER-PUBLIC policy ALLOW_ICMP then permit
	```

These are some of the attributes that can be defined in your policies:

* Source addresses
* Destination addresses
* Applications
* Action (permit/deny/reject/count/log)

Since this is a stateful operation, there is no need to allow return packets (in this case, the echo replies).

In order to allow traffic that is directed to the vSRX itself, use the following command:

```
set security zones security-zone CUSTOMER-PRIVATE interfaces ge-0/0/1.0 host-inbound-traffic system-services all
```

In this case, you are allowing system services traffic (for example, SSH) destined for the IP address configured under the interface `ge-0/0/1.0`.

To allow protocols, such as OSPF or BGP, use the following command:

```
set security zones security-zone trust interfaces ge-0/0/1.0 host-inbound-traffic protocols all
```

## Firewall Filters
By default the IBM Cloud Juniper vSRX allows ping, ssh, and https to itself and drops all other traffic by applying the PROTECT-IN filter to the lo interface.
To configure a new stateless firewall, perform the following procedure:

1. Create firewall filter and term (NB: the following filter will allow only ICMP and drop all other traffic)
	```
	set firewall filter ALLOW-PING term ICMP from protocol icmp
	set firewall filter ALLOW-PING term ICMP then accept
	```
	
2. Apply filter rule to interface (NB: the following will apply the filter to all private network traffic)
	```
	set interfaces ge-0/0/0 unit 0 family inet filter input ALLOW-PING
	```
	

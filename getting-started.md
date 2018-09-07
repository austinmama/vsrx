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

# Getting Started
To get started with IBM Cloud Juniper vSRX Gateway, first determine if your account is linked to Bluemix as this will slightly alter the ordering process. To find out if you have a linked account, navigate to the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){: new_window} in your browser and log in. If your account is unlinked then you will see a "Learn more about Bluemix!" button in the top right.

If your account is unlinked:

1.	Open the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){: new_window} and log into your account.
2.	In the Customer Portal navigation, select **Network > Gateway Appliances**.
3.	From the Gateway Appliances list page, click **Order Gateway**.
4.	From the Order page, select your desired data center from dropdown menu, then choose the desired type of server hardware on which the Juniper vSRX will be provisioned.
5.	On the Order page, select the **High Availability Pair** option from the top of page if desired, then select the Memory size. Next, click the **Juniper** tab under **Operating System**, select **Juniper vSRX 15.x (up to 1 Gbps) Standard** if you picked a single processor server, or **Juniper vSRX 15.x (up to 10 Gbps) Standard** if you picked a dual processor server. Finally, select the desired network uplink speed.
6.	Review your selections then click **Add to Order**, and the order will be verified automatically.
7.	On the Checkout page, if you already own VLANs in the selected data center, select the backend VLANs which need to be protected. Give a hostname and domain name for your server, and check all boxes for the IBM Cloud service terms. Click **Submit Order**.

If your account is linked:

1.	Open [Bluemix ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.bluemix.net/){: new_window} and log into your account.
2.	In the left side navigation, select **Network > Gateway Appliances**.
3.	From the Gateway Appliances list page, click **Order Gateway**.
4.	From the Order page, choose a **Hostname** and **Domain** and then select the **High Availability Pair** option if desired. Next, select your **Location** of choice along with the associated **Pod** and then choose your **Server** and the desired amount of **RAM**. Under **Image**, select **Juniper vSRX 15.x (up to 1 Gbps) Standard** if you picked a single processor server, or **Juniper vSRX 15.x (up to 10 Gbps) Standard** if you picked a dual processor server. Finish choosing any of the optional add-ons, add additional **Storage Disks**, and choose your **Uplink Port Speed**.
5. Review your selections line by line in the **Order Summary**, read and agree to the Third-Party Service Agreements, and then finally click **Provision**.

After your order is approved, the provisioning of your vSRX starts automatically. When the provisioning process is complete, the gateway will appear in the Gateway Appliances list page.

Click the gateway name to open the Gateway Details page. You will find the IP addresses, login username, and password for the device.

## VLANs and the Gateway Appliance's role
A VLAN (virtual LAN) is a mechanism that segregates a physical network into many virtual segments. For convenience, traffic from multiple selected VLANs can be delivered through a single network cable, a process commonly called "trunking."

vSRX is managed in two different interfaces: The vSRX server(s) and the Gateway Appliance fixture. The Gateway Appliance provides you with an interface (GUI and API) for selecting the VLANs you want to associate with your vSRX. Associating a VLAN with a Gateway Appliance reroutes (or "trunks") that VLAN and all of its subnets to your vSRX, giving you control over filtering, forwarding, and protection. Servers in an associated VLAN can only be reached from other VLANs by going through your vSRX; it is not possible to circumvent the vSRX unless you bypass or disassociate the VLAN.

By default, a new Gateway Appliance is associated with two non-removable "transit" VLANs, one each for your public and private networks. These are typically used for administration and can be separately secured by vSRX commands.

The vSRX can only manage VLANs that are associated with it through the Gateway Appliance.

For information on how to manage VLANs from the Gateway Appliances Details screen, refer to the [Manage VLANs](manage-vlans.html) topic.

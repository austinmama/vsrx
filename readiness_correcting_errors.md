---

copyright:
  years: 2018
lastupdated: "2020-02-02"

keywords: correcting, readiness, errors

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
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Correcting readiness errors
{: #correcting-readiness-errors}

## Host (Ubuntu) SSH Connectivity Errors
{: #correct-host_ssh_connectivity}

Many of the Gateway Actions require root SSH access to the private IP address for each host Ubuntu OS. If the precheck readiness SSH connectivity check fails then the action can not proceed. To validate connectivity open an SSH session to the Ubuntu host's private IP using the root credentials listed on the Gateway Details page in the Hardware section. Ensure the SSH session can be established.

<TODO: INSERT SCREENSHOT WITH GW DETAILS PAGE with PRIVATE IP and ROOT passwords highlighted>
  

If the session can not be established ensure:

- A firewall is not blocking SSH access to the private IP.
- The root password listed on the Gateway details page above is the correct password for the root user. If not, then click the device link under the Hardware section and navigate to "Passwords". Use the Actions > Edit credentials action to change the password to match the actual root password on the Ubuntu host.
- The root login may be disabled for the SSH server, or the SSH server may be disabled or stopped.
- The root user account may be disabled on the Ubuntu host.

## Gateway (vSRX) SSH Connectivity Errors
{: #correct-gateway_ssh_connectivity}

Many of the Gateway Actions require root SSH access to the private IP address for the Gateway. If the precheck readiness SSH connectivity check fails then the action can not proceed. To validate connectivity open an SSH session to the vSRX private IP using the root credentials listed on the Gateway Details page. Ensure the SSH session can be established.

<TODO: INSERT SCREENSHOT WITH GW DETAILS PAGE with PRIVATE IP and ROOT passwords highlighted>
  

If the session can not be established ensure:

- The vSRX firewall is not blocking SSH access to the private IP.
- The root password listed on the Gateway details page above is the correct password for the root user. If not, then click the Edit icon the root password and change the password to match the actual root password for the vSRX.
- The root user account may be disabled on the Ubuntu host.





---

copyright:
  years: 2017
lastupdated: "2018-10-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Step by Step: Allowing SSH and Pinging to a Public Subnet
This guide will show how to configure the vSRX with a new interface, zone, and address-book.  As the default action for all traffic is to drop, this guide will show how to set up traffic flows that allow all traffic within the new zone, all traffic from the new zone to the internet, and allow only ssh and ping from the internet to one subnet on the new vlan.

In this example, the following values are used.
```
Public vlan: 1523
Public subnet: 169.47.211.152/29
```

### Creating the new interface, zone, and address-book subnet
```
set interfaces reth3 unit 1523 vlan-id 1523
set interfaces reth3 unit 1523 family inet address 169.47.211.153/29
set security zones security-zone CUSTOMER-PUBLIC interfaces reth3.1523 host-inbound-traffic system-services all
set security address-book global address VSI_PUB_NET 169.47.211.152/29
```

### Creating the new traffic flows
```
set security policies from-zone CUSTOMER-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_INTERNAL description "Allow all traffic within CUSTOMER_PUBLIC zone"
set security policies from-zone CUSTOMER-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_INTERNAL match source-address any
set security policies from-zone CUSTOMER-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_INTERNAL match destination-address any
set security policies from-zone CUSTOMER-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_INTERNAL match application any
set security policies from-zone CUSTOMER-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_INTERNAL then permit

set security policies from-zone CUSTOMER-PUBLIC to-zone SL-PUBLIC policy ALLOW_OUTBOUND description "Allow all outbound traffic from CUSTOMER-PUBLIC to the internet"
set security policies from-zone CUSTOMER-PUBLIC to-zone SL-PUBLIC policy ALLOW_OUTBOUND match source-address any
set security policies from-zone CUSTOMER-PUBLIC to-zone SL-PUBLIC policy ALLOW_OUTBOUND match destination-address any
set security policies from-zone CUSTOMER-PUBLIC to-zone SL-PUBLIC policy ALLOW_OUTBOUND match application any
set security policies from-zone CUSTOMER-PUBLIC to-zone SL-PUBLIC policy ALLOW_OUTBOUND then permit

set security policies from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_PING_SSH description "Allow ping and SSH from the internet to the public subnet"
set security policies from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_PING_SSH match source-address any
set security policies from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_PING_SSH match destination-address VSI_PUB_NET
set security policies from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_PING_SSH match application junos-ssh
set security policies from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_PING_SSH match application junos-ping
set security policies from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC policy ALLOW_PING_SSH then permit
```  

### Checking output and commit
After these changes have been staged, run the following command to double-check what will be committed to the active config:

```
show | compare
```
This is the expected output:

```
[edit security address-book global]
     address SL_PUB_MGMT { ... }
+    address VSI_PUB_NET 169.47.211.152/29;
[edit security policies]
     from-zone SL-PUBLIC to-zone SL-PUBLIC { ... }
+    from-zone CUSTOMER-PUBLIC to-zone CUSTOMER-PUBLIC {
+        policy ALLOW_INTERNAL {
+            description "Allow all traffic within CUSTOMER_PUBLIC zone";
+            match {
+                source-address any;
+                destination-address any;
+                application any;
+            }
+            then {
+                permit;
+            }
+        }
+    }
+    from-zone CUSTOMER-PUBLIC to-zone SL-PUBLIC {
+        policy ALLOW_OUTBOUND {
+            description "Allow all outbound traffic from CUSTOMER-PUBLIC to the internet";
+            match {
+                source-address any;
+                destination-address any;
+                application any;
+            }
+            then {
+                permit;
+            }
+        }
+    }
+    from-zone SL-PUBLIC to-zone CUSTOMER-PUBLIC {
+        policy ALLOW_PING_SSH {
+            description "Allow ping and SSH to public subnet";
+            match {
+                source-address any;
+                destination-address VSI_PUB_NET;
+                application [ junos-ssh junos-ping ];
+            }
+            then {
+                permit;
+            }
+        }
+    }
[edit security zones]
     security-zone SL-PUBLIC { ... }
+    security-zone CUSTOMER-PUBLIC {
+        interfaces {
+            reth3.1523 {
+                host-inbound-traffic {
+                    system-services {
+                        all;
+                    }
+                }
+            }                          
+        }
+    }
[edit interfaces reth3]
+    unit 1523 {
+        vlan-id 1523;
+        family inet {
+            address 169.47.211.153/29;
+        }
+    }
```

After checking the configuration is correct, run `commit` to push the changes to the active configurations

### 
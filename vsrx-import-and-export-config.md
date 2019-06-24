---

copyright:
  years: 2019
lastupdated: "2019-6-14"

keywords: understanding, default, configuration, standalone, ha

subcollection: vsrx

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# Importing/Exporting vSRX Configuration
{: #importing-exporting-vsrx-configuration}

The vSRX upgrade process preserves the configuration, and if the reloads afterwards are done one at a time, then the configuration is still preserved throughout the process. However, it is still strongly recommended to export and backup your vSRX configuration settings before starting the upgrade.

After the upgrade process, for Stand Alone, you should import the original configuration you have saved if you want to restore it. For High Availability, restore configuration manually from your exported file only if for some reason the upgrade fails.

## Considerations
{: #considerations}

* The upgrade process for Standalone and High Availability(HA) are different. Details can be found [here](docs/infrastructure/vsrx?topic=vsrx-upgrading-the-vsrx).
* J-web interface allows you to display, edit, and upload the current configuration quickly and easily without using the Junos OS CLI. Reference [Juniper J-Web User Guide ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.juniper.net/documentation/en_US/junos/topics/concept/J-web-overview.html){:new_window} for more details.
* <a id="InterfaceMapping"></a>An upgrade from the vSRX 15.x 10G offering to vSRX 18.x 10G overring results in changes to the vSRX interface mappings in the configuration file. So when importing original vSRX settings, make sure that the new “interfaces” section is not modified. There are two ways of doing it. You could either import sub-sections other that the “interfaces” section, or you could import the whole config and then manually restore 18.x SR-IOV interfaces.

The new vSRX default interface configuration for both Linux Bridge and SR-IOV needs to be preserved after the import of their configuration's. For example, for SR-IOV the ge interfaces have specific mappings to the host that must be preserved to enable SR-IOV. These interfaces are found in the cli via `show configuration interfaces`. Reference [vsrx default configuration](docs/infrastructure/vsrx?topic=vsrx-understanding-the-vsrx-default-configuration) for more details on SR-IOV mappings.
{: important}

If you prefer using the Junos OS CLI, the following contents provide different methods to export/import your configuration settings, depending on whether you want to export/import the entire configuration or just part of it. To manage the configuration settings, entering CLI mode first, and then run the command `configure` to enter configuration mode. To commit your changes, run the command `commit` under configuration mode.


## Export the Whole vSRX Configuration
{: #export-the-whole-vsrx-configuration}

* Run the command `save /var/tmp/XXX.txt` to save the whole configuration into `/var/tmp`, you can name the txt file as you want. The output should be similar to the following:

```
Wrote 273 lines of configuration to '/var/tmp/SA15DefaultConfig.txt'

[edit]
```
* Copy and save the txt file into your local workspace for later use.

## Export Part of the vSRX Configuration
{: #export-part-of-the-vsrx-configuration}

* Make sure that you are at the top of the configuration tree by running `top` under the configuration mode.
* Run the command `show <section>` to get the current configuration, enclosed in braces. For example, you can run `show interfaces` to show all the interfaces configuration. Or if you prefer to display the output in set mode, run the command `show <section> | display set`. The output should be similar to the following:
```
# show interfaces | display set
set interfaces ge-0/0/0 description PRIVATE_VLANs
set interfaces ge-0/0/0 flexible-vlan-tagging
set interfaces ge-0/0/0 native-vlan-id 925
set interfaces ge-0/0/0 mtu 9000
...

[edit]
```
* Copy and save the output into your local workspace for later use.

## Import the Whole vSRX Configuration
{: #import-the-whole-vsrx-configuration}

[Ensure default interface mappings are preserved](#InterfaceMapping) after import.
{: important}

* After upgrading the vSRX, copy the config file you have saved back to the `/var/tmp` folder.
* Run `load override /var/tmp/XXX.txt` under the configuration mode to replace the entire current configuration with the content that you saved under the `/var/tmp` folder.

## Import Part of the vSRX Configuration
{: #import-part-of-the-vsrx-configuration}

[Ensure default interface mappings are preserved](#InterfaceMapping) after import.
{: important}

* Under configuration mode, run `edit <section>` to go to the configuration tree level that you want.
* Copy the configuration settings you have saved and run the command `load merge terminal relative` to merge the configuration with the current one.
* Paste (control + v) the content.
* Hit “enter” to go to a new line and then type control + D to end input. The output should be similar to the following:
```
# load merge terminal relative
[Type ^D at a new line to end input]
family inet {
            filter {
                input PROTECT-IN;
            }
        }
load complete


[edit interfaces lo0 unit 0]
```
* If you want to replace the configuration instead of merge it, you could delete the configuration first by running `delete` under this configuration tree level and then do a `load merge terminal relative` to copy and paste your previous configuration. 
* If you prefer to edit the configuration in set mode, you could run `load set terminal` instead of `load merge terminal relative`. And then copy and paste the content you saved in set mode. Note: make sure that you always run `load set terminal` at the top.
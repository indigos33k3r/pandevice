
Splunk for Palo Alto Networks App
=================================

## Description ##

Field extractions and sample reports,
and dashboards for the Palo Alto
Networks Firewall

#### Latest Version ####

* Splunk Version: 6.x
* App Version: 4.1
* Last Modified: Apr 2013
* Authors:
    * Monzy Merza - Splunk, Inc.
    * Brian Torres-Gil - Palo Alto Networks

#### Version Compatibility ####

Splunk 6.x -- Palo Alto Networks App 4.x  
Splunk 5.x -- Palo Alto Networks App 3.x

#### Support ####

Further documentation can be found at:  
https://github.com/PaloAltoNetworks-BD/SplunkforPaloAltoNetworks/wiki

For fastest response to support, setup, help or feedback,
please click the __Ask a Question__ button at http://apps.splunk.com/app/491

For bugs or feature requests, you can also open an issue on github at 
https://github.com/PaloAltoNetworks-BD/SplunkforPaloAltoNetworks/issues

## Quick Start Guide ##

Install the app:

- Unpack the tar ball into `$SPLUNK_HOME/etc/apps`
- Restart Splunk

Note: If upgrading from a previous version, please read the __Upgrade Notes__ below.

### Setup Screen and Custom Commands ###

The first time you run the app from the web ui, you will be presented with a setup screen. The credentials are only needed if you wish to use the `pantag`, `panblock`, `panupdate` custom commands. The WildFire API is only needed if you are a WildFire subscriber and want Splunk to index WildFire analysis reports from the cloud when a malware sample is analyzed.  These credentials will be stored in Splunk using encryption the same way other Splunk credentials are stored.

If you do not wish to use these extra features, you can enter garbage values.

### To get the firewall data into Splunk ###

IMPORTANT: When you configure the input port, you must set the sourcetype of the firewall data to pan_log and the index to pan_logs.  This can be done from the Web UI or the CLI.  Then, configure the firewall to set traffic to Splunk.

#### From the Splunk Web UI ####

- Navigate to Manager -> Data Inputs -> UDP -> New
- Set the UDP port (Palo Alto Networks firewalls default to port 514)
- Set sourcetype: From list
- Select source type From list: pan_log
- Click on More settings
- Index: pan_logs

For details: http://www.splunk.com/base/Documentation/latest/admin/MonitorNetworkPorts

#### From the CLI via inputs.conf ####

- Edit `$SPLUNK_HOME/etc/apps/SplunkforPaloAltoNetworks/local/inputs.conf` 

Example:  (Palo Alto Networks firewalls default to udp port 514)

    [udp://514]
    index= pan_logs
    connection_host = ip
    sourcetype = pan_log
    no_appending_timestamp = true

#### Configure the Firewall ####

On the Palo Alto Networks firewall or Panorama management center, create a Log Forwarding object to send desired syslogs to the Splunk Server. Refer to the Palo Alto Networks documentation for details on log forwarding.  https://live.paloaltonetworks.com/community/documentation

Note: It can take up to 5 minutes for new data to show up in the dashboards.  Palo Alto Networks devices have a variety of different logs including traffic, threat, url filtering, malware, etc. This app works with the all the default log types. Customized log types may not work, if they are not defined in the Palo Alto Networks syslog configuration documentation (PANOS-Syslog-Integration-TN-RevM).

### Upgrade Notes ###

Starting in version 4.1 of this app, all of the dashboards use the Splunk 6 Datamodel feature, which allows for pivot of Palo Alto Networks data and better control and acceleration of summary indexes used by the dashboards.  This replaces the TSIDX feature from Splunk 5.

If you are upgrading the app from a pre-4.1 version to 4.1 or higher, then you may delete the TSIDX files that were generated by the previous version of the app.  To delete the TSIDX files, look under $SPLUNK_HOME$/var/lib/splunk/tsidxstats/ and remove any directories that start with `pan_`.  There could be up to 10 directories.

After upgrade to 4.1 or higher, Splunk will backfill the datamodel with historic data up to 1 year old.  It may take some time for historic data to show up in the dashboards, but it will be available in the pivot interface and search immediately.  The time range for historic data to be available in the dashboards can be adjusted in the datamodel accelerations settings.

## What's new in this version ##

Version 4.1

If upgrading from a previous version, please read the __Upgrade Notes__ above.

- PAN-OS Data model including acceleration
- Data model accelerated dashboards (replaces TSIDX-based dashboards)
- New command: `pantag` - tag IP addresses on the firewall into Dynamic Address Groups
- IP Classification - add metadata to your CIDR blocks, classifying them as internet/external/dmz/datacenter/etc.
- Applipedia change notifications and highlighting - know when Palo Alto Networks releases new application signatures and if those applications are on your network

## Installing from Git ##

This app is available on [Splunk Apps](http://apps.splunk.com/app/491) and [Github](https://github.com/PaloAltoNetworks-BD/SplunkforPaloAltoNetworks).  Optionally, you can clone the github repository to install the app.
From the directory `$SPLUNK_HOME/etc/apps/`, type the following command:

    git clone https://github.com/PaloAltoNetworks-BD/SplunkforPaloAltoNetworks.git SplunkforPaloAltoNetworks


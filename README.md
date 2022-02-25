# telegraf-plugins

My plugins for [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/).

## `cloudinsights-snmp-switch.conf`

With this plugin you can monitor a switch with [Cloud Insights](https://cloud.netapp.com/cloud-insights) via SNMP. This has been tested with a Cisco Nexus 9372. You need to tweak it for your switch model. 

1. Put the file `cloudinsights-snmp-switch.conf` into `/etc/telegraf/telegraf.d`

2. Change

    * `agents = ["<switch_ip_address>"]`
    * `url = "https://<your_tenant>.c01.cloudinsights.netapp.com/rest/v1/lake/ingest/influxdb"`
    * `X-CloudInsights-ApiKey = "<your_netapp_cloud_insights_apikey>"`

### Requirements

#### net-snmp

Install snmp e.g. `apt-get install snmp`

#### MIBs

You can put the required MIBs into `/etc/telegraf/.snmp/mibs`

For this example I used

* `CISCO-ENTITY-FRU-CONTROL.MIB`
* `CISCO-IF-EXTENSION-MIB`
* `CISCO-SMI.MIB`
* `IF-MIB`
* `RFC1213-MIB`
* `SNMPv2-SMI`

### Troubleshooting

Use `snmpwalk` to check if it is possible to get metrics from the switch

```sh
snmpwalk -v2c -m +ALL -c public <switch_ip_address>
```

Check logfiles

* `/var/log/telegraf/telegraf.log`
* `/var/log/telegraf/telegraf-snmp-switch.json`

## `cloudinsights-sqlserver.conf`

With this plugin you can monitor a MS SQL Server with [Cloud Insights](https://cloud.netapp.com/cloud-insights). 

Be aware of the *counter_types* when configurihttps://troubleshootingsql.com/2011/03/03/what-does-cntr_type-mean/ng dashboards/widgets! A good explanation can be found [here](https://troubleshootingsql.com/2011/03/03/what-does-cntr_type-mean/)

1. Put the file `cloudinsights-sqlserver.conf` into `C:\Program Files\telegraf\telegraf.d`

2. Change

    * `url = "https://<your_tenant>.c01.cloudinsights.netapp.com/rest/v1/lake/ingest/influxdb"`
    * `X-CloudInsights-ApiKey = "<your_netapp_cloud_insights_apikey>"`
    * `<SQL_Server_IP>`
    * `<SQL_Server_User>`
    * `<SQL_Server_User_Password>`
    
        ```toml
        servers = [
          "Server=<SQL_Server_IP>;Port=1433;User Id=<SQL_Server_User>;Password=<SQL_Server_User_Password>;app name=telegraf;log=1;",
        ]
        ```

### Requirements

* Install the **Windows** Data Collector with Cloud Insights
* I recommend the following setting for the `cloudinsights-default.conf`
    * `interval = "60s"`
    * `metric_batch_size = 5000`
    * and for troubleshooting `debug = true`

## General Troubleshooting

`/usr/bin/telegraf --config /etc/telegraf/telegraf.conf --config-directory /etc/telegraf/telegraf.d --test`
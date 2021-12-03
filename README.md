# telegraf-plugins

My plugins for [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/).

## `cloudinsights-snmp-switch.conf`

With this plugin you can monitor a switch with [Cloud Insights](https://cloud.netapp.com/cloud-insights) via SNMP. This has been tested with a Cisco Nexus 9372. You need to tweak it for your switch model. 

1. Put the file `cloudinsights-snmp-switch.conf` into `/etc/telegraf/telegraf.d`

2. Change

    * `agents = ["<switch_ip_address>"]`
    * `url = "https://<your_tenant>.c01.cloudinsights.netapp.com/rest/v1/lake/ingest/influxdb"`
    * `X-CloudInsights-ApiKey = "<your_netapp_cloud_insights_apikey"`

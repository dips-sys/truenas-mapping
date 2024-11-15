# truenas-mapping
[TruenNAS Core](https://www.truenas.com/truenas-core/) can export performance
 metrics to a remote Graphite server. However, if you use Prometheus for system 
 monitoring and you want to ingest the TrueNAS data you need to run an instance 
 of [graphite_exporter](https://github.com/prometheus/graphite_exporter) to 
 ingest the data. The mapping file provided here, maps the Graphite metrics 
 paths to Prometheus names and labels. Specify the path to the file with the 
 `--graphite.mapping-config=truenas-mapping.yml` argument when starting 
 graphite_exporter.
 
Altough graphite_exporter warns about possible performance issues due to 
backtracking in the match expressions, this mapping allows to retrieve labeled 
data. For example, `truenas_aggregation_cpu{job="truenas", cpu=~"cpu-average"}` 
will retrieve idle, interrupt, nice, system and user metrics in one query. 
Network activity can be retrieved by 
`rate(truenas_interface_if_octets{job="truenas", interface=~"ix0|bridge0", action="rx"}[$__rate_interval])`

I took my inspiration from in this [older Forum Post](https://www.truenas.com/community/threads/mapping-of-freenas-data-sent-to-graphite_exporter-part-of-prometheus.80948/#post-560770), 
but I found it a bit diffcult to navigate through the data as labels are hardly
used.
 
## TrueNAS configuration
Check the Graphite Separate Instances option in System -> Reporting.

By default TrueNAS Core sends the metrics to port 2003.

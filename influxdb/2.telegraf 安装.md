# telegraf 安装

### 配置文件
```
mkdir /home/influx/telegraf
vim  /home/influx/telegraf/telegraf.conf
```
```
[[outputs.influxdb_v2]]
 ## The URLs of the InfluxDB cluster nodes.
 ##
 ## Multiple URLs can be specified for a single cluster, only ONE of the
 ## urls will be written to each interval.
 ## urls exp: http://127.0.0.1:8086
 urls = ["http://ip:8086"]
 
 ## Token for authentication.
 token = "my-super-secret-auth-token"
 
 ## Organization is the name of the organization you wish to write to; must exist.
 organization = "my-org"
 
 ## Destination bucket to write into.
 bucket = "telegraf"
# Read metrics about system load & uptime
[[inputs.system]]
  # no configuration
[[inputs.statsd]]
  max_tcp_connections = 250
  tcp_keep_alive = false
  service_address = ":8125"
  delete_gauges = true
  delete_counters = true
  delete_sets = true
  delete_timings = true
  percentiles = [90.0]
  metric_separator = "."

```
### 启动 telegraf
```
docker run -d --name=telegraf \
    -p 8125:8125/udp \
    -v /home/influx/telegraf/telegraf.conf:/etc/telegraf/telegraf.conf:ro \
    -v /:/hostfs:ro \
    -e HOST_ETC=/hostfs/etc \
    -e HOST_PROC=/hostfs/proc \
    -e HOST_SYS=/hostfs/sys \
    -e HOST_VAR=/hostfs/var \
    -e HOST_RUN=/hostfs/run \
    -e HOST_MOUNT_PREFIX=/hostfs \
    telegraf
```
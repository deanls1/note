# influxdb 安装

![influxdb oss](https://github.com/deanls1/note/blob/main/static/influx%202.0.png)
```
mkdir /home/influx/
mkdir /home/influx/influxdb
```

```
docker run -d -p 8086:8086 \
       --name influxdb \
       --restart unless-stopped \
       -v /home/influx/influxdb/data:/var/lib/influxdb2 \
       -v /home/influx/influxdb/config:/etc/influxdb2 \
       -e DOCKER_INFLUXDB_INIT_MODE=setup \
       -e DOCKER_INFLUXDB_INIT_USERNAME=admin \
       -e DOCKER_INFLUXDB_INIT_PASSWORD=admin123 \
       -e DOCKER_INFLUXDB_INIT_ORG=my-org \
       -e DOCKER_INFLUXDB_INIT_BUCKET=telegraf \
       -e DOCKER_INFLUXDB_INIT_RETENTION=1w \
       -e DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=my-super-secret-auth-token \
       -d influxdb 
```
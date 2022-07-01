# 安装prometheus
## 下载镜像
~~~
docker pull prom/prometheus:latest
~~~
## 配置文件
~~~
mkdir -p /home/docker/prometheus
vim /home/docker/prometheus/prometheus.yml
~~~
~~~
global:

  scrape_interval:     15s #抓取数据间隔
  evaluation_interval: 15s #评估规则间隔

alerting: #alertmanager相关配置
  alertmanagers:
  - static_configs:
    - targets:
       - 172.17.0.18:9093 #alertmanager地址

rule_files: #加载规则文件 
  - "/etc/prometheus/rule/*.yml"

scrape_configs:
  - job_name: 'Hosts'
    static_configs:
      - targets: ['172.17.0.18:9200']   #被控端地址-主机监控
  - job_name: 'Containers'
    static_configs:
      - targets: ['172.17.0.18:18104']   #被控端地址-容器监控    
~~~
## 启动命令
~~~
docker run -d -p 9090:9090 \
-v /home/docker/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml \
-v /etc/localtime:/etc/localtime \
-v /home/docker/prometheus/rule:/etc/prometheus/rule \
--name prometheus \
prom/prometheus
~~~

# 安装granafa
## 下载命令
~~~
docker pull grafana/grafana:latest 
~~~
## 启动命令
~~~
docker run -d -i -p 3000:3000 \
-v "/etc/localtime:/etc/localtime" \
-e "GF_SERVER_ROOT_URL=http://grafana.server.name" \
-e "GF_SECURITY_ADMIN_PASSWORD=admin123" \
grafana/grafana
~~~

## 主机模板ID：8919
## 容器模板ID：11558

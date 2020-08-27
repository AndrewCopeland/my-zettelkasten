# 1598542084 conjur-prometheus-config
#conjur #prometheus #setup

First I setup a conjur master contianer:
```bash
CONJUR_NAME=conjur-master
IMAGE_NAME=captainfluffytoes/dap:11.4.0
CONJUR_HOSTNAME=conjur-master
ADMIN_PASSWORD=CYberark11@@
CONJUR_ACCOUNT=conjur

docker network create conjur
docker container run -d --name $CONJUR_NAME --network conjur --security-opt=seccomp:unconfined -p 443:443 -p 5432:5432 -p 1999:1999 $IMAGE_NAME
docker exec $CONJUR_NAME evoke configure master --accept-eula --hostname $CONJUR_HOSTNAME --admin-password $ADMIN_PASSWORD $CONJUR_ACCOUNT
```

Second I created a prometheus config file `/tmp/prometheus.yml` with the content of:
```yaml
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 1m
  external_labels:
    monitor: codelab-monitor
scrape_configs:
- job_name: prometheus
  scrape_interval: 5s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - localhost:9090
- job_name: conjur
  scrape_interval: 5s
  metrics_path: /health
  scheme: http
  static_configs:
  - targets:
    - localhost:444
```

Third I started the prometheus container:
```bash
docker run -d --network conjur --name prometheus -p 9090:9090 -v /tmp/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```

Currently I am recieveing the following error message from the conjur target:
```
"INVALID" is not a valid start token
```

I think the metrics endpoint has to expose data in a specific manner, following this documentation:
https://prometheus.io/docs/instrumenting/exposition_formats/


So as of now, it looks like conjur does not support the ability to retrieve metrics with Prometheus.
This ruby library implements this specific `exposition format`: https://github.com/prometheus/client_ruby



## Links

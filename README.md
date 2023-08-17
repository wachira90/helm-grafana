# grafana

```sh
helm template app prometheus > prom.yaml

helm template app node-exporter > node-exporter.yaml

helm template app grafana > grafana.yaml
```

## docker-compose

```yml
version: "3.3"

services:
  # Use admin/admin as username/password  docker pull grafana/grafana:9.5.1
  grafana:
    container_name: grafana-localhost
    image: grafana/grafana:9.4.7
    restart: always
    ports:
      - 3000:3000
    environment:
      GF_SERVER_ROOT_URL: http://172.16.1.5:3000

  prometheus:
    container_name: prometheus
    image: prom/prometheus:v2.43.0
    restart: always
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    command:
      - "--config.file=/etc/prometheus/prometheus.yml"
    ports:
      - 9090:9090

  prometheus-node-exporter:
    container_name: prometheus-node-exporter
    image: prom/node-exporter:v1.5.0
    restart: always
    depends_on:
      - prometheus
    ports:
      - 9100:9100
```
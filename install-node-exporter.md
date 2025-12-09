# 安装 Node Exporter

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.10.2/node_exporter-1.10.2.linux-amd64.tar.gz
tar -xvf node_exporter-1.10.2.linux-amd64.tar.gz
sudo mv node_exporter-1.10.2.linux-amd64/node_exporter /usr/local/bin/
```


## systemd 配置：

```bash
sudo tee /etc/systemd/system/node_exporter.service <<EOF
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=root
ExecStart=/usr/local/bin/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

启动：

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```

访问：

```
http://<server-ip>:9100/metrics
```


## Prometheus 配置抓取 Node Exporter

在 prometheus.yml 里加：

```yaml
scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets:
        - '192.168.50.10:9100'
        - '192.168.50.20:9100'
```

## Grafana 直接导入官方 Dashboard

Grafana → Create → Import → 输入 ID 即可


| Dashboard 名称               | ID   | 内容                               |
|-----------------------------|------|------------------------------------|
| Node Exporter Full          | 1860 | 完整 CPU/Mem/Disk/Network VM 仪表板 |
| Node Exporter for Prometheus| 8919 | 更轻量                              |

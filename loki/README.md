
Promtail → Loki → Grafana Setup Guide


**Step 1:** Deploy Loki & Grafana via Docker Compose in seperate VM

```bash
mkdir -p loki-data/{index,boltdb-cache,chunks,compactor,wal}
sudo chown -R 10001:10001 loki-data
```
Then start your stack:

`docker compose up -d`

Make sure your docker-compose.yml includes Loki configurations with proper volume mappings and network configuration.


**Step 2:** Deploy Promtail in Kubernetes

Update the Promtail DaemonSet ConfigMap to point to your Loki endpoint.
```bash
clients:
  - url: http://<LOKI_HOST>:3100/loki/api/v1/push
```
Apply Promtail using your YAML manifest


**Step 3:** Create a Sample Pod to Generate Logs

**Step 4:** Configure Grafana with Loki

- Log in to Grafana (http://<GRAFANA_HOST>:3000)
- Go to Settings > Data Sources
- Add Loki as a new data source using your configured URL: http://<LOKI_HOST>:3100
- Grafana > Explore > Select data dource > Label filters: Pod=your_pod_name > Run Query



[Ref-1](https://grafana.com/docs/loki/latest/send-data/promtail/installation/)
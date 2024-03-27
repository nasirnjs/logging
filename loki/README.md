helm search repo loki
helm show values grafana/loki-stack > values.yaml
cat values.yaml
helm install --values values.yaml loki  grafana/loki-stack


k get secrets loki-grafana -o jsonpath={.data.admin-password}

echo "a202RmhtMU1uemJ0RktsbU1mdG9ZN2E4ZUozQ2RrS1N3Z0o5aFpOVQ==" | base64 --decode

loki  k get secrets loki-promtail -o jsonpath="{.data.promtail\.yaml}"  | base64 --decode

k get secrets loki-promtail -o jsonpath="{.data.promtail\.yaml}"  | base64 --decode > promtail.yaml

---

helm upgrade --install --create-namespace -n portainer portainer portainer/portainer \
    --set service.type=LoadBalancer \
    --set tls.force=true \
    --set persistence.storageClass=nfs-client
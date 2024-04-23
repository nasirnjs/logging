
helm install elasticsearch \
 --set service.type=LoadBalancer \
 --set volumeClaimTemplate.storageClassName=nfs-client \
 --set persistence.labels.enabled=true elastic/elasticsearch -n efk
 ===================================================================
 kubectl create secret generic elasticsearch-certs \
  --from-file=ca.crt=ca.crt \
  --from-file=elasticsearch.crt=elasticsearch.crt \
  --from-file=elasticsearch.key=elasticsearch.key \
  --namespace=efk
```bash
helm repo list
helm search repo elastic
helm show values elastic/elasticsearch > elasticsearch-values.yaml
helm install elasticsearch elastic/elasticsearch -f values-backup/elasticsearch-values.yaml
```

==============================
kubectl create secret generic fluentbit-certs \
  --from-file=ca.crt=ca.crt \
  --from-file=fluentbit.crt=fluentbit.crt \
  --from-file=fluentbit.key=fluentbit.key \
  --namespace=efk

```bash
helm repo list
helm search repo fluent
helm show values fluent/fluent-bit > fluentbit-values.yaml
helm install fluent-bit fluent/fluent-bit -f values-backup/fluentbit-values.yaml
```

kubectl get secrets --namespace=efk elasticsearch-master-credentials -ojsonpath='{.data.username}' | base64 -d
User Name:	elastic

kubectl get secrets --namespace=efk elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
Password: wwNaWAYMKii423y7

=============================================
```bash
helm repo list
helm search repo kibana
helm show values elastic/kibana > kibana-values.yaml
helm install kibana elastic/kibana -f values-backup/kibana-values.yaml
```





https://medium.com/@tech_18484/simplifying-kubernetes-logging-with-efk-stack-158da47ce982






**Certificate Authority**

```bash
openssl genrsa -out ca.key 2048
openssl req -new -key ca.key -out ca.csr -subj "/CN=KUBERNETES-CA"
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt -days 365
```


```bash
openssl genrsa -out elasticsearch.key 2048
openssl req -new -key elasticsearch.key -out elasticsearch.csr -subj "/CN=elasticsearch"
openssl x509 -req -in elasticsearch.csr -CA ca.crt -CAkey ca.key -out elasticsearch.crt -days 365
```

```bash
openssl genrsa -out fluentbit.key 2048
openssl req -new -key fluentbit.key -out fluentbit.csr -subj "/CN=fluentbit"
openssl x509 -req -in fluentbit.csr -CA ca.crt -CAkey ca.key -out fluentbit.crt -days 365

```


helm show values fluent/fluent-bit > fluentbit-values.yaml



curl -XGET https://172.17.17.45:9200 --cacert ca.crt --cert fluentbit.crt --key fluentbit.key -v
# Example configuration for monitored rabbitmq on a kubernetes cluster
## Prerequisites
To run this project, you must first install the following tools:
* Docker
* Kubernetes cluster
* Helm 3
* nxing ingress (otherwise change/delete ingress configrations)
* Have a storage class named `local-fs-storage` (or find and replace the name/comment persistence out)
## Setup
To setup the stack, run following commands:
```
helm install -f prometheus-grafana/prometheus-values.yaml prometheus-grafana-test prometheus-community/kube-prometheus-stack
helm install filebeat elk/filebeat/
helm install logstash elk/logstash/
helm install elasticsearch elk/elastic/
helm install kibana elk/kibana/
helm install monitored-rabbitmq rabbitmq/
kubectl apply -f rabbitmq/instance.yaml
``` 
## Access the rabbitmq web ui
To access the UI, start port-forwarding (or create an ingress):
```
kubectl port-forward service/rabbit-mq-test-cluster 15672
```

You will be greeted with a login form. To find out the default username and password, you have to check the secret (the pattern is `INSTANCE_NAME-default-user`):
```
kubectl get secret rabbit-mq-test-cluster-default-user -oyaml
```
You should see a base64 encoded username and password that you use to get access to the UI.
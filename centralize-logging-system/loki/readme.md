# cloudshell-repo

Grafana loki setup guide

## kube-prometheus-stack
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

## grafana
helm repo add grafana https://grafana.github.io/helm-charts

## Create a namespace for monitoring 
kubectl create ns monitoring 

## install kube-prometheus-stack
helm upgrade --install prom-stack prometheus-community/kube-prometheus-stack --values prometheus-config.yaml -n monitoring

Add default prometheus datasource and addtional data source loki

## Install promtail logging agent

helm upgrade --install promtail grafana/promtail --values promtail-values.yaml -n monitoring

## Install loki 
helm upgrade --install loki grafana/loki-distributed -n monitoring



helm upgrade --install loki grafana/loki-stack --values loki-values.yaml -n monitoring


### Grafana credentials
If you are using Prometheus Operator then user/pass is:

user: admin
pass: prom-operator

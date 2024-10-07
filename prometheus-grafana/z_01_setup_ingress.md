# Use ingress for grafana  

## Step 1: Prerequsites: Install Ingress Controller

## Step 2: Add ip to dns 

```
grafana.tln1  A 167.45.23.21
```


## Step 3: adajust values.yml 

```
fullnameOverride: prometheus

alertmanager:
  fullnameOverride: alertmanager

grafana:
  fullnameOverride: grafana
  ingress:
    enabled: true
    # must be set, otherwice it is not working
    helm upgrade prometheus prometheus-community/kube-prometheus-stack -f values.yml --install --namespace monitoring --create-namespace --version 65.1.0
    hosts:
      - grafana.tln1.t3isp.de

kube-state-metrics:
  fullnameOverride: kube-state-metrics

prometheus-node-exporter:
  fullnameOverride: node-exporter

```

## Step 4: Run upgrade or install of installation 

  * Important: Please be careful about different versions (59.1 -> 65.1. have breaking changes)
  * So it is more safe to delete the crds firstly or evaluate which crds are present 

```
helm -n monitoring list
helm upgrade prometheus prometheus-community/kube-prometheus-stack -f values.yml --install --namespace monitoring --create-namespace --version 65.1.0

```

## Step 5: 

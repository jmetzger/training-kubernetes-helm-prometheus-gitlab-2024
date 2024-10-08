# Prometheus with Grafana (Install with helm)

  * using the kube-prometheus-stack (recommended !)
  * https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack

## Step 1: Prepare values-file  

```
cd
mkdir -p manifests 
cd manifests 
mkdir monitoring 
cd monitoring 
```

```
vi values.yml 
```

```
fullnameOverride: prometheus

alertmanager:
  fullnameOverride: alertmanager

grafana:
  fullnameOverride: grafana

kube-state-metrics:
  fullnameOverride: kube-state-metrics

prometheus-node-exporter:
  fullnameOverride: node-exporter
```

## Step 2: Install with helm 

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack -f values.yml --namespace monitoring --create-namespace --version 65.1.0
```

## Step 2.1 Check ob alles läuft 

```
kubectl -n monitoring get all 
```



## Step 3: Connect to prometheus from the outside world 

### Step 3.1: Start proxy to connect (to on Linux Client)

```
# this is shown in the helm information 
helm -n monitoring get notes prometheus

# Get pod that runs prometheus 
kubectl -n monitoring get service 
kubectl -n monitoring port-forward svc/prometheus-prometheus 9091 &

```

### Step 3.2: Start a tunnel in (from) your local-system to the server 

   * each tln using her or his tln-nr. e.g. 2 -> 9092 

```
# ip is the ip of the client machne (jump host) 
ssh -L 9091:localhost:9091 tln1@164.92.129.7
```

![image](https://github.com/user-attachments/assets/5df83fb0-047d-4b30-8d01-89d20ce67d49)


### Step 3.3: Open prometheus in your local browser 

```
# in browser with tln - nr, e.g. tln2 -> 9092
http://localhost:9091
```

## Step 4: Connect to the grafana from the outside world 

### Step 4.1: Start proxy to connect 

```
# Do the port forwarding 
# Adjust your pods here
kubectl -n monitoring get pods | grep grafana 
kubectl -n monitoring port-forward grafana-56b45d8bd9-bp899 3000 &
```

### Step 4.2: Start a tunnel in (from) your local-system to the server 

```
# 164.. -> ip of lcient machine (jump-server) 
ssh -L 3000:localhost:3000 tln1@164.92.129.7
```







## References:

  * https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/README.md
  * https://artifacthub.io/packages/helm/prometheus-community/prometheus

  

# Setup Blackbox exporter 

## Prerequisites 

  * prometheus setup with helm

## Step 1: Setup

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install my-prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter --version 9.0.1 --namespace monitoring --create-namespace

```

## Step 2: Find SVC 

```
kubectl -n monitoring get svc | grep blackbox
```

```
my-prometheus-blackbox-exporter   ClusterIP   10.245.183.66    <none>        9115/TCP              
```


## Step 3: Test with Curl 

```
kubectl run -it --rm curltest --image=curlimages/curl -- sh 
```

```
# Testen nach google in shell von curl
curl http://my-prometheus-blackbox-exporter.monitoring:9115/probe?target=google.com&module=http_2xx
```

```
# Looking for metric 
probe_http_status_code 200
```

## Step 4: Test apple-service with Curl 

```
kubectl run -it --rm curltest --image=curlimages/curl -- sh 
```

```
# From within curlimages/curl pod 
curl http://my-prometheus-blackbox-exporter.monitoring:9115/probe?target=apple-service.default&module=http_2xx
```


## Step 5: Scrape Config (We want to get all services being annotated with example.io/should_be_probed = true

```
adjust values.yaml for helm chart
```


```
prometheus:
  prometheusSpec:
    additionalScrapeConfigs:
    - job_name: "blackbox-microservices"
      metrics_path: /probe
      params:
        module: [http_2xx]
      # Autodiscovery through kube-api-server 
      # https://prometheus.io/docs/prometheus/latest/configuration/configuration/#kubernetes_sd_config
      kubernetes_sd_configs:
      - role: service
      relabel_configs:
        # Example relabel to probe only some services that have "example.io/should_be_probed = true" annotation
        - source_labels: [__meta_kubernetes_service_annotation_example_io_should_be_probed]
          action: keep
          regex: true
        - source_labels: [__address__]
          target_label: __param_target
        - target_label: __address__
          replacement:  my-prometheus-blackbox-exporter:9115
        - source_labels: [__param_target]
          target_label: instance
        - action: labelmap
          regex: __meta_kubernetes_service_label_(.+)
        - source_labels: [__meta_kubernetes_namespace]
          target_label: app
        - source_labels: [__meta_kubernetes_service_name]
          target_label: kubernetes_service_name
```

## Step 6: Test with relabeler 

 * https://relabeler.promlabs.com

```


```

## Step 7: Scrapeconfig einbauen 

```
# von kube-prometheus-grafana in values und ugraden 
 helm upgrade prometheus prometheus-community/kube-prometheus-stack -f values.yml --install --namespace monitoring --create-namespace --version 65.1.0
```

## Step 8: annotation in service einfügen 

```
kind: Service
apiVersion: v1
metadata:
  name: apple-service
  annotations:
    example.io/should_be_probed: "true"

spec:
  selector:
    app: apple
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5678 # Default port for image
```


```
kubectl apply -f service.yml
```

## Step 9: Look into Status -> Discovery Services and wait

  * blackbox services should now appear under blackbox_microservices
  * and not being dropped

## Step 10: Unter http://64.227.125.201:30090/targets?search= gucken

  * .. ob das funktioniert

## Step 11: Hauptseite (status code 200) 

![image](https://github.com/user-attachments/assets/8969e578-6f97-4246-954f-6fa0fad26020)


  * Metrik angekommen `?
  * http://64.227.125.201:30090/graph?g0.expr=probe_http_status_code&g0.tab=1&g0.display_mode=lines&g0.show_exemplars=0&g0.range_input=1h

## Step 12: pod vom service stoppen

```
kubectl delete pod apple-app
```

## Step 13: status_code 0


  * Metrik angekommen `?
  * http://64.227.125.201:30090/graph?g0.expr=probe_http_status_code&g0.tab=1&g0.display_mode=lines&g0.show_exemplars=0&g0.range_input=1h

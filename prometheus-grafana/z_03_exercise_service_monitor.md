# Exercise for using a service monitor with a new app

## ServiceMonitor: Step 1: Create Test app 

```
mkdir testapp
```

```
nano main.js
```

```
const express = require("express");
const prometheus = require("prom-client");

const app = express();

const counter = new prometheus.Counter({
  name: "TotalRequests",
  help: "Total requests made to the service",
  labelNames: ["uri"]
});

const counterMiddleware = (req, res, next) => {
  if (req.path !== "/prometheus") {
    counter.inc({uri: req.path});
  }
  next();
};

app.use(counterMiddleware);

app.get("/prometheus", (req, res) => {
  res.set("Content-Type", prometheus.register.contentType);
  prometheus.register.metrics().then(metrics => res.send(metrics));
});

app.get("*", (req, res) => res.send("Spacelift Prometheus demo app"));

app.listen(80, () => console.log("Ready"))
```

```
npm install express prom-client
```

## ServiceMonitor: Step 2: Create Dockerfile, build and upload 

```
nano Dockerfile
```

```
FROM node:18
WORKDIR /app

COPY *.json ./
RUN npm ci

COPY *.js ./

ENTRYPOINT ["node", "main.js"]
```

```
# user dockertrainereu
# pass: ask trainer 
docker login
docker build -t dockertrainereu/testapp:latest .
docker push -t dockertrainereu/testapp:latest
```

## ServiceMonitor: Step 3: Create Deployment / Service 

```
cd
mkdir -p manifests
cd manifests
mkdir testapp
cd testapp
```

```
nano deploy.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: testapp-prometheus-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: testapp-prometheus-demo
  template:
    metadata:
      labels:
        app: testapp-prometheus-demo
    spec:
      containers:
        - name: app
          image: dockertrainereu/testapp:latest
          ports:
            - name: http
              containerPort: 80

---

apiVersion: v1
kind: Service
metadata:
  name: testapp-prometheus-demo
  labels:
    app: testapp-prometheus-demo
spec:
  selector:
    app: testapp-prometheus-demo
  ports:
    - name: http
      port: 80
```

```
kubectl apply -f .
```

## ServiceMonitor: Step 4: Connect to service 

```
kubectl run -it --rm podtest --image busybox
```

```
# Do this 5 x 
wget -O - http://testapp-prometheus-demo
wget -O - http://testapp-prometheus-demo/prometheus

```

## ServiceMonitor: Step 5: Rollout ServiceMonitor 

```
nano testapp-service-monitor.yaml
```

```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: testapp-prometheus-servicemonitor
  labels:

# According to:
# prometheus - server object 
# kubectl -n monitoring get prometheus -o yaml | grep -A 4 serviceMonitorSelector 
#
# serviceMonitorSelector:
#      matchLabels:
#        release: prometheus

    release: prometheus 
spec:
  endpoints:
    - port: http
      path: /prometheus
      interval: 15s
  selector:
    matchLabels:
      app: testapp-prometheus-demo
```

```
kubectl apply -f .
```

## ServiceMonitor: Step 6: Check if config was changed in prometheus 

```
# port-forwarding and evtl. ssh-tunnel needs to be set 
http://localhost:9090
```


  * https://spacelift.io/blog/prometheus-operator#how-to-set-up-servicemonitor-and-metrics-sources





## Reference 

  * https://spacelift.io/blog/prometheus-operator


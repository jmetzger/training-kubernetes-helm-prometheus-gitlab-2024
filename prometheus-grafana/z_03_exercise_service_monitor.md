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

## ServiceMonitor: Step 6: Check in the logs, if it was detected 

```
kubectl -n monitoring logs prometheus-prometheus-prometheus-0 | grep -i testapp
```


## ServiceMonitor: Step 7: Check if config was changed in prometheus 

```
# port-forwarding and evtl. ssh-tunnel needs to be set 
http://localhost:9090
```

```
http://localhost:9090/config # Menu: Status -> Con![image](https://github.com/user-attachments/assets/09e00e98-0ce2-4eec-9564-5379ce8de08c)
figuration
```

```
# Find the corresponding block -> job
# This got automatically created, by having the object and detecting it. 
scrape_configs:
- job_name: serviceMonitor/default/testapp-prometheus-servicemonitor/0
```

## ServiceMonitor: Step 8: Check in Service Discovery (Raw Data)

  * It shows, it has detected the endpoints of the service 
  * It shows the labels it has found
  * All labels starting with __ will not be written to database
    * We will need to rewrite them 

![image](https://github.com/user-attachments/assets/4ad8fd06-2fba-4ef7-9b19-d211e0f2682d)

## ServiceMonitor: Step 9: Check in Targets (Rewritten Data)

  * http://localhost:9090/targets?search= (Menu -> Status -> Targets)
  * Shows the relabeled information (That is the way it is save in db and can be used, e.g. by grafana)
  * (If you unfold it, also the discovered labels)

  ![image](https://github.com/user-attachments/assets/445d5fcd-ed56-4976-94bd-255ad0caef4d)

## ServiceMonitor: Step 10: Show the data 

  * Go to main page: http://localhost:9090
  * Search for: Total Requests, press Execute 
  * Click on it: You will see the metrics collected

![image](https://github.com/user-attachments/assets/a2858cc6-e257-40ed-8ed4-64c00150b114)

  * Now click on graph -> you will see the information as graph

![image](https://github.com/user-attachments/assets/d86f9071-42f2-462d-9201-2a96b933600d)

  * Now rerun a call to the pod like earlier:


```
kubectl run -it --rm podtest --image busybox
```

```
# Do this 5 x 
wget -O - http://testapp-prometheus-demo
wget -O - http://testapp-prometheus-demo/prometheus

```

  * Try again after 1 minute (Main Page -> Total Requests -> Table 
    * You will see the last, increased value 
  * Also Try: Main Page -> Total Requests -> Graph
    * You will find a spike at the end of the Graph 

![image](https://github.com/user-attachments/assets/8898bb72-115a-47cd-829a-697045f31e4a)

## Service Monitor: Step 10: Add to grafana 

```
Open grafana:

e.g.
http://grafana.tln1.t3isp.de
user: admin
pass: prom-operator
```

```
Click on: Dashboards -> New Dashboard
```
![image](https://github.com/user-attachments/assets/8c382221-4c9a-4d58-8687-9f967f7f9f00)


```
Click on: Add Visualization
```

```
Select Data Source: Prometheus
```

![image](https://github.com/user-attachments/assets/6eca32fd-20a1-4622-91a5-1d5accd8e51d)

```
1. Search for your TotalRequests metric in the “Select metric” dropdown within the query builder at the bottom of the screen
2. Click apply
```

![image](https://github.com/user-attachments/assets/56fd3ba8-6a66-411c-ada4-d75bec003bf5)

```
1. Click: Run Queries to see how it goes
2. Click: Apply (top right on page)
3. Click on the save item, and give the dashboard a name, e.g. Service Testapp
(now you can find it under dashboards -> scroll down.
```

## Service Monitor: Step 11: Add it to the home screen  

```
Star the dashboard
(Being in the dashboard, press the start at the top left of the screen)
```

![image](https://github.com/user-attachments/assets/f7e9e8a6-bb75-4efb-9657-edd7798f25de)

```
Set it for your user:
(under: Home Dashboard)
```

![image](https://github.com/user-attachments/assets/631c967e-d9a6-413c-8788-77fd8bf60fc6)

```
Or under Organization (then it is for the whole organization)
```

![image](https://github.com/user-attachments/assets/fbb351f0-a72c-4310-8246-484a112d4bc6)



```
# Alternative would be setting it for Teams
```

## Reference 

  * https://spacelift.io/blog/prometheus-operator#how-to-set-up-servicemonitor-and-metrics-sources

# Secret Example (Mariadb) 

## Schritt 1: secret 

```
cd 
mkdir -p manifests
cd manifests
mkdir cftest 
cd cftest 
nano 01-secret.yml 
```

```
## 01-secret.yml
kubectl create secret generic mariadb-secret --dry-run=client -o yaml --from-literal=MARIADB_ROOT_PASSWORD=11abc432 > 01-secret.yml
```

```
kubectl apply -f .
kubectl get cm
kubectl get cm mariadb-secret -o yaml
```


## Schritt 2: Deployment 
```
nano 02-deploy.yml
```

```
#deploy.yml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mariadb-deployment
spec:
  selector:
    matchLabels:
      app: mariadb
  replicas: 1 
  template:
    metadata:
      labels:
        app: mariadb
    spec:
      containers:
      - name: mariadb-cont
        image: mariadb:10.11
        envFrom:
        - secretRef:
            name: mariadb-secret

```

```
kubectl apply -f .
```


# MariaDB 

  * How to persistently use mariadb with a storage class / driver nfs.csi.

## Step 1: PVC, Configmap, Deployment 

```
cd
mkdir -p manifests
cd manifests
mkdir -p storage-do
cd storage-do
nano 01-pvc.yaml
```


```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nfs-dynamic-mariadb
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: do-block-storage-retain
```

```
nano 02-configmap.yml
```

```
## 02-configmap.yml
kind: ConfigMap
apiVersion: v1
metadata:
  name: mariadb-configmap
data:
  # als Wertepaare
  MARIADB_ROOT_PASSWORD: 11abc432
```

```
nano 03-deploy.yml
```

```
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
        - configMapRef:
            name: mariadb-configmap
        volumeMounts:
        - name: persistent-storage
          mountPath: "/var/lib/mysql"
          readOnly: false
      volumes:
      - name: persistent-storage
        persistentVolumeClaim:
          claimName: pvc-nfs-dynamic-mariadb
```

```
kubectl apply -f .
```

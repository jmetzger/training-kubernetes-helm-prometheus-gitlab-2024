# Example kubectl cp and job 

## How to it ?

```
# Step 1: Run job, that wait for a file in a specific folder
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: alpine
        command: ["sh",  "somescript.sh"]
      restartPolicy: Never
  backoffLimit: 4


```

´´´
# Step 2: Copy file to job with kubectl cp 
kubectl get jobs 

# tar must be installed in container 
kubectl cp /tmp/foo_dir <some-pod>:/tmp/bar_dir


## Reference 

  * https://kubernetes.io/docs/concepts/workloads/controllers/job/

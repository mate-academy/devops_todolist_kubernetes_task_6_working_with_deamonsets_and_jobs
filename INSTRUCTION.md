# INSTRUCTION

## Deploy DaemonSet and CronJob to Kubernetes cluster

1. ### Apply manifests:
``` bash
kubectl apply -f .infrastructure/
```

2. ### Validate
Check resources
``` bash
kubectl get all -n mateapp
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

3. ### Validate DaemonSet

Check pods:

``` bash
kubectl get pods -n mateapp
``` 

View logs:
``` bash
kubectl logs <daemonset-pod-name> -n mateapp
```

**_Logs should show curl requests every 5 seconds._**

4. ### Validate CronJob

**_Wait ~4 minutes, then:_**
``` bash
kubectl get jobs -n mateapp
kubectl get pods -n mateapp
```

View logs:
``` bash
kubectl logs <cronjob-pod-name> -n mateapp
```

Expected output:
``` bash

Date
Health OK

```
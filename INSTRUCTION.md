1. Verify if you have access to the Kubernetes cluster:

```bash
kubectl cluster-info
```

2. Deploy DaemonSet:

```bash
kubectl apply -f daemonset.yml
```

3. Deploy CronJob:

```bash
kubectl apply -f cronjob.yml
```

4. Check if DaemonSet is running:

```bash
kubectl get daemonset -n mateapp
```

5. Check if CronJob exists:

```bash
kubectl get cronjob -n mateapp
```

6. Verify Pod's logs, created by CronJob:

```bash
kubectl get jobs -n mateapp
```

7. Verify logs of DaemonSet (pods):

```bash
kubectl logs -l app=curl-daemon -n mateapp
```

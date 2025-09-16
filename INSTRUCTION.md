1. Set namespace
```bash
kubectl config set-context --current --namespace=mateapp
```

2. Deploy DaemonSet and CronJob:

```bash
kubectl apply -f daemonset.yml cronjob.yml
```

3. Verify Pod's logs, created by CronJob:

```bash
kubectl get jobs
```

4. Verify logs of DaemonSet:

```bash
kubectl logs -l app=todoapp-curl-service
```
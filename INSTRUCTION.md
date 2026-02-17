# Deployment and validation

## 1. Deploy manifests

```bash
kubectl create namespace mateapp --dry-run=client -o yaml | kubectl apply -f -
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## 2. Validate DaemonSet

```bash
kubectl get daemonset -n mateapp
kubectl get pods -n mateapp -l app=todoapp-curl-daemon -o wide
```

Check logs (there should be repeated requests every ~5 seconds):

```bash
kubectl logs -n mateapp -l app=todoapp-curl-daemon --tail=30
```

## 3. Validate CronJob

```bash
kubectl get cronjob -n mateapp
kubectl describe cronjob todoapp-healthcheck -n mateapp
```

Wait for schedule (`*/4 * * * *`) or trigger one run manually:

```bash
kubectl create job --from=cronjob/todoapp-healthcheck todoapp-healthcheck-manual -n mateapp
kubectl get jobs -n mateapp
```

Check logs from CronJob-created Job:

```bash
kubectl logs -n mateapp job/todoapp-healthcheck-manual
```

For scheduled runs, find the latest Job and view logs:

```bash
kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp
kubectl logs -n mateapp job/<latest-job-name>
```

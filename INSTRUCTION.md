## Check DaemonSet status

```
kubectl get daemonset -n mateapp
```

## Check DaemonSet logs

```
kubectl logs -l app=todoapp -n mateapp --tail=10
```

## Check CronJob status

```
kubectl get cronjob -n mateapp
```

## Check CronJob logs

```
kubectl logs $(kubectl get pods -n mateapp -o jsonpath="{.items[0].metadata.name}") -n mateapp --tail=10
```
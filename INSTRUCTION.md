To deploy daemonset.yml and cronjob.yml run:
```
# DaemonSet
kubectl apply -f .infrastructure/daemonset.yml
# CronJob
kubectl apply -f .infrastructure/cronjob.yml
```

To see logs from daemonset.yml and cronjob.yml:
```
# To see all pods
kubectl get pods -n mateapp -o wide

# To see logs of daemon
kubectl logs {name_of_daemon_pod} -n mateapp

# To see logs of cronjob
kubectl logs {name_of_cronjob_pod} -n mateapp
```
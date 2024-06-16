### Use these command:

`kubectl apply -f .infrastructure/daemonset.yml, -f .infrastructure/cronjob.yml`

### Check logs for the `daemonset` and `cronjob`

`kubectl get daemonset -n mateapp`

`kubectl get cronjob -n mateapp`

## Deployment Instructions

1. Apply the `mateapp` namespace:
   ```bash
   kubectl create namespace mateapp
   ```

2. Deploy the `DaemonSet`:
   ```bash
   kubectl apply -f daemonset.yml
   ```

3. Deploy the `CronJob`:
   ```bash
   kubectl apply -f cronjob.yml
   ```

## Validation Instructions

1. Check `DaemonSet` logs:
   ```bash
   kubectl logs -n mateapp -l app=todo-monitor -f
   ```

2. Check `CronJob` logs:
   ```bash
   kubectl logs -n mateapp job/$(kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-1].metadata.name}')
   ```

3. Verify `DaemonSet` status:
   ```bash
   kubectl get daemonset -n mateapp
   ```

4. Verify `CronJob` status:
   ```bash
   kubectl get cronjob -n mateapp
   ```
# Deployment Instructions for DaemonSet and CronJob

## DaemonSet Deployment
1. **Apply the DaemonSet Manifest**:
   ```bash
   kubectl apply -f .infrastructure/daemonset.yml
   ```

2. **Verify the DaemonSet**:
   ```bash
   kubectl get daemonsets -n mateapp
   ```

3. **Check Logs for DaemonSet**:
   ```bash
   kubectl logs -l app=mateapp-daemon -n mateapp
   ```

## CronJob Deployment
1. **Apply the CronJob Manifest**:
   ```bash
   kubectl apply -f .infrastructure/cronjob.yml
   ```

2. **Verify the CronJob**:
   ```bash
   kubectl get cronjobs -n mateapp
   ```

3. **Check Logs for CronJob**:
   ```bash
   kubectl logs job/$(kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-1:].metadata.name}') -n mateapp
   ```

## Validation
- **DaemonSet**:
  - Ensure logs show periodic `curl` requests to the `todoapp` service every 5 seconds.
- **CronJob**:
  - Ensure logs show successful `curl` requests to `/api/health` endpoint every 4 minutes.

## Namespace
Both manifests belong to the `mateapp` namespace. Ensure the namespace exists before deploying.

## Additional Notes
- Replace `todoapp-clusterip` with the actual ClusterIP service name if different.
- Ensure the `todoapp` service is running and accessible.
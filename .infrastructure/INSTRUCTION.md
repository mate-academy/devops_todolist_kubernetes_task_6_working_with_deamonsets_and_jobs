# ToDo App Monitoring Deployment

## Depoyment Instructions

1. **Ensure `todoapp` Service Exists**  
   Verify that the `todoapp-svc` ClusterIP service is running in the `mateapp` namespace.

2. **Apply Manifests**  
   Deploy the DaemonSet and CronJob:
   ```sh
   kubectl apply -f daemonset.yml
   kubectl apply -f cronjob.yml
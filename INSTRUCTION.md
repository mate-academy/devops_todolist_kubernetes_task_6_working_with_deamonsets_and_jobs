### The instructions on how to deploy daemonset.yml and cronjob.yml to the cluster.
```bash
   kubectl apply -k .\.infrastructure\
   ```
### The instructions on how to validate the solution daemonset and cronjob.
    kubectl logs <name_pod_daemonset> --tail=100 | findstr curl
    kubectl logs <name_cronejob_pod> --tail=5 | findstr Health

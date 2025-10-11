## How to deploy DaemonSet and CronJob to the cluster.

### DaemonSet:
    kubectl apply -f .infrastructure/daemonset.yml -n mateapp

### CronJob:
    kubectl apply -f .infrastructure/cronjob.yml -n mateapp

## How to validate the solution.

### DaemonSet:
- List pods created by the DaemonSet:
    
    
    kubectl get pods -n mateapp -l app=daemonset

- check logs for all pods by label:


    kubectl logs -l app=daemonset -n mateapp


### CronJob:
- List jobs created by the CronJob:

    
    kubectl get jobs -n mateapp

- Check logs for a specific job pod:


    kubectl logs -l job-name=cronjob -n mateapp

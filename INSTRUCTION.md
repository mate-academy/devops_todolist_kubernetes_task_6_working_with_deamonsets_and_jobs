## How to deploy DaemonSet and CronJob to the cluster.

### Namespace applying:
    kubectl apply -f .infrastructure/namespace.yml

### DaemonSet deploying:
    kubectl apply -f .infrastructure/daemonset.yml -n mateapp

### CronJob deploying:
    kubectl apply -f .infrastructure/cronjob.yml -n mateapp

## How to validate the solution.

### DaemonSet:
- List pods created by the DaemonSet:
    
    
    kubectl get pods -n mateapp -l app=daemonset -o wide

- DaemonSet check:


    kubectl get daemonset -n mateapp

- Pod logs:


    kubectl logs -n mateapp <pod-name> -f

- check logs for all pods by label:


    kubectl logs -l app=daemonset -n mateapp


### CronJob:
- List jobs created by the CronJob:

    
    kubectl get jobs -n mateapp

- Get cronjob:


    kubectl get cronjob -n mateapp -o yaml

- Check logs for a specific job pod:


    kubectl logs -l job-name=cronjob -n mateapp

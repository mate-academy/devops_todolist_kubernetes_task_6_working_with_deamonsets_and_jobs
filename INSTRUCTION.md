# Instructions on how to deploy.

## Deployment of DaemonSet:
```
kubectl apply -f ./.infrastructure/daemonset.yml
```
#### Verify deployment: DaemonSet
```
kubectl get pods -n mateapp
```
#### To view the logs of DaemonSet pods, use the command:
```
kubectl logs <pod-name> -n mateapp
```


## Deploying CronJob:
```
kubectl apply -f ./.infrastructure/cronjob.yml
```

#### Checking CronJob:
```
kubectl get cronjobs -n mateapp
```
#### To view the logs for a specific pod created by a CronJob, run:
```
kubectl logs <cronjob-pod-name> -n mateapp
```
#### You should see requests to the /api/health endpoint.

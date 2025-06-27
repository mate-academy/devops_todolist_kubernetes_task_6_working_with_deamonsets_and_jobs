1) Apply manifests to create the `todoapp` namespace and deploy the resources:
```bash
kubectl apply -f ./.infrastructure/namespace.yml
kubectl apply -f ./.infrastructure/
```
2) Validate the deployment by checking the status of the DaemonSet and CronJob:
```bash
kubectl get daemonset -n todoapp
kubectl get cronjob -n todoapp
```
3) Check the logs of the DaemonSet to ensure it is executing the `curl` command every 5 seconds:
```bash
kubectl get daemonsets -n todoapp
kubectl get pods -n todoapp -l app=busybox
kubectl logs -n todoapp <daemonset-pod-name>
```
4) Check the logs of the CronJob to ensure it is executing the `curl` command every 4 minutes:
```bash
kubectl get jobs -n todoapp
kubectl get pods -n todoapp -l job-name=<cronjob-name>
kubectl logs -n todoapp <cronjob-pod-name>
```
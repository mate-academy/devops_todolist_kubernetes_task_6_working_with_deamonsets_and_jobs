# DaemonSet and CronJob Instructions

## DaemonSet

Apply the DaemonSet:
kubectl apply -f .infrastructure/daemonset.yml

Check the DaemonSet:
kubectl get daemonset -n todoapp

Check the Pods created by the DaemonSet:
kubectl get pods -n todoapp

Check the DaemonSet Pod logs:
kubectl logs <daemonset-pod-name> -n todoapp

## CronJob

Apply the CronJob:
kubectl apply -f .infrastructure/cronjob.yml

Check the CronJob:
kubectl get cronjobs -n todoapp

Check Jobs created by the CronJob:
kubectl get jobs -n todoapp

Check the Pods created by the Jobs:
kubectl get pods -n todoapp

Check the CronJob Pod logs:
kubectl logs <cronjob-pod-name> -n todoapp
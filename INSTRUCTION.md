# DaemonSet and CronJob Instructions

## DaemonSet

Apply the DaemonSet:
kubectl apply -f .infrastructure/daemonset.yml

Check the DaemonSet:
kubectl get daemonset -n mateapp

Check the Pods created by the DaemonSet:
kubectl get pods -n mateapp

Check the DaemonSet Pod logs:
kubectl logs <daemonset-pod-name> -n mateapp

## CronJob

Apply the CronJob:
kubectl apply -f .infrastructure/cronjob.yml

Check the CronJob:
kubectl get cronjobs -n mateapp

Check Jobs created by the CronJob:
kubectl get jobs -n mateapp

Check the Pods created by the Jobs:
kubectl get pods -n mateapp

Check the CronJob Pod logs:
kubectl logs <cronjob-pod-name> -n mateapp
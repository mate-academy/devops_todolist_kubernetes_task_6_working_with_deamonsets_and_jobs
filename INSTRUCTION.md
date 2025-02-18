Apply the DaemonSet Manifest - kubectl apply -f daemonset.yml
Apply the CronJob Manifest - kubectl apply -f cronjob.yml


Steps to validate that everything works

Check if the DaemonSet is running correctly:
- kubectl get daemonset -n mateapp
- kubectl get pods -n mateapp -o wide

This will show the last 10 log lines of the DaemonSet's Pods => kubectl logs <pod-name> -n mateapp --tail=10

Check if the CronJob is running correctly:
- kubectl get cronjob -n mateapp
- kubectl get jobs -n mateapp
The <jobs> command shows if new Jobs are created every 4 minutes.

This confirms that the CronJob Pods are actually calling the /api/health endpoint:
- kubectl logs -l job-name=<cronjob-name> -n mateapp --tail=10

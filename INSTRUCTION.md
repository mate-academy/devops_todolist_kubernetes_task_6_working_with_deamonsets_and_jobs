### Check that DaemonSet and CronJob have started:

kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp

### Check DaemonSet logs:
kubectl logs -n mateapp -l app=daemonset_app
Expected output:


Curling todoapp...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    12  100    12    0     0     24      0 --:--:-- --:--:-- --:--:--    41
Readiness OK

### Check CronJob logs

Get the list of CronJob pods:
kubectl get pods -n mateapp

Then check logs for the CronJob pod:
kubectl logs <pod-name> -n mateapp


Sample output:
Sun Jun  1 15:47:12 UTC 2025
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0     18      0 --:--:-- --:--:-- --:--:--    45
Health OK
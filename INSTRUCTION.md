Apply the namespaces:

command: "kubectl apply -f .infrastructure/namespace.yml"


Deploy the daemonset and the cronjob to the `mateapp` namespace:

command:
1: "kubectl apply -f .infrastructure/daemonset.yml"
2: "kubectl apply -f .infrastructure/cronjob.yml"


Check that everything is created:

command:
1: "kubectl get daemonset -n mateapp"
2: "kubectl get cronjob -n mateapp"
3: "kubectl get pods -n mateapp"



Get the pod names:

command: " kubectl get pods -n mateapp"

Check the daemonset logs. A `200` line should appear every 5 seconds:

command: "kubectl logs <curl-daemonset-pod> -n mateapp"

Wait 4 minutes for the cronjob to run, then check its logs. The response of `/api/health` should be there:

command: "kubectl logs <curl-cronjob-pod> -n mateapp"
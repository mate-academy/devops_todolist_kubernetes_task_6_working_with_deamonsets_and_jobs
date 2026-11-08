# how to deploy daemonset.yml and cronjob.yml to the cluster.
cd /path/to/project/.infrastructure
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml

# how to validate the solution. (Logs for the daemonset and cronjob should be present)
# daemonset logs check
kubectl get pods -n mateapp
kubectl logs -f todoapp-abc12 -n mateapp
# todoapp-abc12 should be replaced with your name

# cronjobs logs check
kubectl get cronjobs -o wide
kubectl get jobs -n mateapp
kubectl logs health-check-28476958-vnkgh
# health-check-28476958-vnkgh should be replaced with your name
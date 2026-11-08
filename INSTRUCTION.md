# how to deploy daemonset.yml and cronjob.yml to the cluster.
cd /path/to/project/.infrastructure
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml

# how to validate the solution. (Logs for the daemonset and cronjob should be present)
kubectl logs -f daemonset/todoapp -n mateapp
kubectl get cronjobs -o wide
kubectl get jobs
kubectl logs health-check-28476958-vnkgh
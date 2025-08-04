kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
kubectl logs mateapp-daemonset
kubectl logs mateapp-cronjob
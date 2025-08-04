kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
kubectl logs mateapp-deamonset
kubectl logs mateapp-cronjob
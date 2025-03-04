deploy:
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
Logs: 
kubectl get daemonset
kubectl get cronjob
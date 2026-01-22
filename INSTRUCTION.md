kubectl apply -f daemonset.yml -n mateapp
kubectl apply -f cronjob.yml -n mateapp

Get daemon and cronjob pods name:
kubectl get pods -n mateapp

kubectl logs <daemon pod name>
kubectl logs <cronjob pod name>
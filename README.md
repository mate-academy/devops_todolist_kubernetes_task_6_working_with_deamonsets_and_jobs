how to deploy daemonset.yml and cronjob.yml to the cluster.
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f hpa.yml
kubectl apply -f busybox.yml
kubectl apply -f clusterIp.yml
kubectl apply -f nodeport.yml
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
check pods
kubectl get pods -n mateapp
check daemonset
kubectl get daemonset -n mateapp
check cronjob
kubectl get jobs -n mateapp
check logs
kubectl logs name_of_pod -n mateapp
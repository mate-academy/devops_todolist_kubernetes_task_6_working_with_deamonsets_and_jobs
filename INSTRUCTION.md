kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml

kubectl get pods
cubectl logs todoapp-daemonset-jdfb5
cubectl logs todoapp-daemonset-ktd8k 
cubectl logs todoapp-cronjob-29254396-qx42r
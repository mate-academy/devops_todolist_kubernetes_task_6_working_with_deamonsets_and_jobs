kubectl apply -f daemonset.yml

kubectl get pods -o wide

kubectl get nodes -o wide

kubectl exec -it -n mateapp busybox -- sh
curl http://<node-ip-1>:30008
curl http://<node-ip-2>:30008

kubectl apply -f cronJob.yml

kubectl get cronjobs -o wide

kubectl get pods

kubectl logs <cronjob-pod>
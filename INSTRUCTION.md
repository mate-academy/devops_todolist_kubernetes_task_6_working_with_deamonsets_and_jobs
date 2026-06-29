kubectl config set-context --current --namespace=mateapp

kubectl apply -f daemonset.yml

kubectl get pods -o wide
copy <daemonset-pod-name>

kubectl logs <daemonset-pod-name>

kubectl get nodes -o wide

kubectl exec -it -n mateapp busybox -- sh
curl http://<node-ip-1>:30008
curl http://<node-ip-2>:30008

kubectl apply -f cronJob.yml

kubectl get cronjobs -o wide

kubectl get pods -o wide
copy <cronjob-pod-name>

kubectl logs <cronjob-pod-name>
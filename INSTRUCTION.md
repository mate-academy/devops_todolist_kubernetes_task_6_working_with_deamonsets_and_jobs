kubectl apply -f deamonset.yml

kubectl get nodes -o wide

kubectl exec -it -n mateapp busybox -- sh
curl http://<internal-ip>:30008

kubectl apply -f cronJob.yml

kubectl get pods

kubectl logs <cronjob-pod>
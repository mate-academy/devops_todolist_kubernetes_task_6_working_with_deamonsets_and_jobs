1. How to deploy daemonset.yml and cronjob.yml to the cluster.
Need run commands:
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml

2. how to validate the solution. (Logs for the daemonset and cronjob should be present)
Need run commands:
kubectl get pods -o wide
Find name pods witch start with my-server-****
kubectl logs my-server-****
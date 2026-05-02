# Deployment Instructions
## Deploy to cluster
```bash
kubectl apply -f .infrastructure/service.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
Validate DaemonSet
kubectl get pods -n mateapp -l app=kube2py
kubectl logs -l app=kube2py -n mateapp --tail=10
Validate CronJob
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
kubectl logs -l job-name=health-check -n mateapp --tail=10
Expected results

DaemonSet pods should be running and curling the health endpoint every 5 seconds
CronJob should run every 4 minutes and keep 10 successful / 5 failed jobs in history



Але попередньо виправ помилки в файлах:
1. **daemonset.yml** — образ `busyboxplus:curl` замість `curlimages/curl:latest`
2. **service.yml** — namespace `mateapp`, targetPort `8000`
3. **cronjob.yml** — URL `todoapp-service.mateapp.svc.cluster.local`


Потім задеплоїш і перевіриш логи!
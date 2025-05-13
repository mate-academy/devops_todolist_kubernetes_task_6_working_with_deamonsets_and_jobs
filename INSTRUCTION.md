# Instructions to deploy DaemonSet and CronJob

## 1. Create namespace (якщо ще не існує)
```bash
kubectl create namespace mateapp
2. Apply DaemonSet
bash
kubectl apply -f daemonset.yml
3. Apply CronJob
bash
kubectl apply -f cronjob.yml
Validation Instructions
DaemonSet logs (перевірити, що виконується curl)
bash
kubectl get pods -n mateapp -l app=todoapp-curl
kubectl logs <POD_NAME> -n mateapp
CronJob logs
bash
kubectl get jobs -n mateapp
kubectl get pods -n mateapp --selector=job-name=<JOB_NAME>
kubectl logs <POD_NAME> -n mateapp
## ✅ КРОК 4. Застосування

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
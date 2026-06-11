# Instructions

## Deploy manifests

kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml

## Validate DaemonSet

# Проверь что поды запущены на каждой ноде
kubectl get pods -n mateapp -l app=todoapp-daemonset -o wide

# Смотри логи DaemonSet пода
kubectl logs -n mateapp -l app=todoapp-daemonset

## Validate CronJob

# Проверь что cronjob создан
kubectl get cronjob -n mateapp

# Проверь запущенные jobs
kubectl get jobs -n mateapp

# Смотри логи последнего job
kubectl logs -n mateapp -l job-name=<имя_job>
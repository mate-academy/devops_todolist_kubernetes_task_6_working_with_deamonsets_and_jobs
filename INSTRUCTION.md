# Якщо mateapp ще не існує — створіть його:
kubectl create namespace mateapp

# Деплой DaemonSet
kubectl apply -f daemonset.yml

# Перевір статус DaemonSet:
kubectl get daemonset -n mateapp

# Перевір створені поди:
kubectl get pods -n mateapp -l app=todoapp-daemon

# Деплой CronJob
kubectl apply -f cronjob.yml

# Перевір створення CronJob:
kubectl get cronjob -n mateapp

# Валідація рішення

# Перевір логи DaemonSet
# DaemonSet запускає контейнер на кожному вузлі і виконує curl кожні 5 секунд.
kubectl get pods -n mateapp -l app=todoapp-daemon

# Потім подивіться логи будь-якого пода:
kubectl logs <daemonset-pod-name> -n mateapp

# Очікуваний вивід у логах:
Pinging ToDo app service...
<html>... або Health check output ...</html>

# Перевір логи CronJob
# CronJob запускається кожні 4 хвилини, перевіряючи /api/health ендпоінт.
# Перевір останні запуски:
kubectl get jobs -n mateapp

# Подивіться поди, створені CronJob:
kubectl get pods -n mateapp | grep todoapp

# Вивід логів останнього запуску:
kubectl logs <cronjob-pod-name> -n mateapp

# Очікуваний вивід:
Checking ToDo app health endpoint...
Health check failed
або
Checking ToDo app health endpoint...
{"status":"ok"}

# Видалення ресурсів
# Якщо потрібно очистити кластер:
kubectl delete -f daemonset.yml
kubectl delete -f cronjob.yml
kubectl delete namespace mateapp
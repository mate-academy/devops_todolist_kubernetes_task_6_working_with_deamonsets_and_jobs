# Створення namespace
# Якщо mateapp ще не існує:
kubectl create namespace mateapp

# Деплой DaemonSet
# Файл: .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/daemonset.yml

# Перевір статус DaemonSet:
kubectl get daemonset -n mateapp

# Перевір створені поди:
kubectl get pods -n mateapp -l app=todoapp

# Деплой CronJob
# Файл: .infrastructure/cronjob.yml
kubectl apply -f .infrastructure/cronjob.yml

# Перевір створення CronJob:
kubectl get cronjob -n mateapp

# Валідація рішення
# Перевір логи DaemonSet
# DaemonSet запускає контейнер на кожному вузлі та виконує curl кожні 5 секунд.
kubectl get pods -n mateapp -l app=todoapp

# Переглянь логи будь-якого пода:
kubectl logs <daemonset-pod-name> -n mateapp

# Очікуваний результат:
Pinging ToDo app service...
<html>... або Health check output ...</html>

# Перевір логи CronJob
# CronJob запускається кожні 4 хвилини, перевіряючи /api/health ендпоінт.
# Перевір останні job-и:
kubectl get jobs -n mateapp

# Подивись поди, створені CronJob:
kubectl get pods -n mateapp | grep todoapp

# Переглянь логи останнього пода:
kubectl logs <cronjob-pod-name> -n mateapp

# Очікуваний вивід:
Checking ToDo app health endpoint...
{"status":"ok"}
або
Health check failed

# Видалення ресурсів
# Щоб видалити ресурси з кластера:
kubectl delete -f .infrastructure/daemonset.yml
kubectl delete -f .infrastructure/cronjob.yml
kubectl delete namespace mateapp
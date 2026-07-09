# Інструкція з розгортання та перевірки інфраструктури


1. Попереднє розгортання додатка

Перед запуском нових компонентів основний простір імен `mateapp` та всі базові маніфести програми успішно застосовані:

kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterip.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/hpa.yml

2. Розгортання DaemonSet та CronJob

kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml

3. Інструкція щодо перевірки рішення

DaemonSet автоматично створює под на кожній доступній ноді й надсилає циклічні запити до служби todoapp-service кожні 5 секунд.
Перевірка статусу подів та перегляд журналів (логів) DaemonSet:
kubectl get pods -n mateapp -l app=todoapp-daemonset
kubectl logs -n mateapp -l app=todoapp-daemonset --tail=20

Перевірка роботи CronJob
kubectl get cronjobs -n mateapp
kubectl get jobs -n mateapp

Перегляд журналів (логів) для успішно виконаного тестового завдання:
kubectl logs -n mateapp job/manual-test-2
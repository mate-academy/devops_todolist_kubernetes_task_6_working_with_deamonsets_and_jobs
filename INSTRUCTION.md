нструкція з розгортання DaemonSet та CronJob

Ця інструкція описує кроки для розгортання системних Job-ів та DaemonSet, які перевіряють стан додатка todoapp у просторі імен mateapp.

1. Припущення та Передумови

Всі необхідні маніфести (daemonset.yml та cronjob.yml) знаходяться у поточній робочій директорії.

Важливо: Вважається, що у кластері вже існує Service з назвою todoapp у просторі імен mateapp, який маршрутизує трафік до todoapp Deployment.

Простір імен mateapp має бути створений.

kubectl create namespace mateapp


2. Розгортання Ресурсів

Крок 2.1. Розгортання DaemonSet

Застосуйте маніфест daemonset.yml, який запустить агент моніторингу на кожній робочій ноді, перевіряючи Liveness Probe (/api/liveness/) кожні 5 секунд.

kubectl apply -f daemonset.yml -n mateapp


Крок 2.2. Розгортання CronJob

Застосуйте маніфест cronjob.yml, який запланує виконання перевірки стану Readiness Probe (/api/readiness/) кожні 4 хвилини.

kubectl apply -f cronjob.yml -n mateapp


3. Валідація Розгортання

Крок 3.1. Валідація DaemonSet (Безперервне виконання)

Переконайтеся, що DaemonSet розгорнув поди на всіх робочих нодах, і вони успішно надсилають запити Liveness Probe.

Перевірка DaemonSet: Переконайтеся, що кількість бажаних (DESIRED) та поточних (CURRENT) подів збігається.

kubectl get daemonset -n mateapp


Перевірка Pods: Перегляньте список подів, створених DaemonSet.

kubectl get pods -n mateapp -l app=todoapp-health-agent -o wide


Перевірка логів (Валідація виконання): Перегляньте логи одного з DaemonSet Pods, щоб підтвердити, що curl виконується кожні 5 секунд, і бачить статус SUCCESS.

# Знаходимо ім'я першого Pod DaemonSet
POD_NAME=$(kubectl get pods -n mateapp -l app=todoapp-health-agent -o jsonpath='{.items[0].metadata.name}')

# Перегляд логів в режимі реального часу (-f)
kubectl logs $POD_NAME -n mateapp -f


Крок 3.2. Валідація CronJob (Заплановане виконання)

Перевірте, що CronJob створює Jobs, які успішно завершуються після перевірки Readiness Probe.

Перевірка CronJob: Переконайтеся, що CronJob зареєстрований і має правильний розклад (*/4 * * * *).

kubectl get cronjobs -n mateapp -o wide


Перевірка Jobs: Перевірте, чи створюються Jobs згідно з розкладом та чи вони завершуються (COMPLETIONS має бути 1/1).

kubectl get jobs -n mateapp


Перевірка логів (Валідація виконання): Знайдіть останній успішно виконаний Job і перегляньте його логи, щоб підтвердити успішний виклик /api/readiness/.

# Отримання імені останнього Job, створеного CronJob
JOB_NAME=$(kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-1:].metadata.name}')

# Перегляд логів цього Job
kubectl logs -n mateapp $JOB_NAME


Ви маєте побачити повідомлення Readiness Check SUCCESS (Status: 200) або подібне, залежно від того, як ви налаштували логи в cronjob.yml.
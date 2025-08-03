ІНСТРУКЦІЯ З РОЗГОРТАННЯ РІШЕННЯ
	1.	Упевніться, що в кластері існує namespace з назвою mateapp. Якщо ні — створіть його командою:

kubectl create namespace mateapp
	2.	Застосуйте файл daemonset.yml для створення DaemonSet:

kubectl apply -f daemonset.yml
	3.	Застосуйте файл cronjob.yml для створення CronJob:

kubectl apply -f cronjob.yml

⸻

ІНСТРУКЦІЯ З ПЕРЕВІРКИ РІШЕННЯ
	1.	Перевірте, що DaemonSet створив по одному Pod-у на кожному вузлі:

kubectl get daemonset my-daemonset -n mateapp
kubectl get pods -n mateapp -l app=mateapp
	2.	Перегляньте логи одного з Pod-ів DaemonSet:

kubectl logs -n mateapp <імʼя-podʼа>
	3.	Перевірте, що CronJob запланований і виконується кожні 4 хвилини:

kubectl get cronjob my-cronjob -n mateapp
kubectl get jobs -n mateapp
	4.	Після запуску CronJob знайдіть відповідний pod і перегляньте логи:

kubectl get pods -n mateapp –-selector=job-name=<імʼя-jobʼа>
kubectl logs -n mateapp <імʼя-podʼа>
	5.	У логах повинно бути видно запит до ендпоінта /api/health або повідомлення Health check failed, якщо сервіс недоступний.
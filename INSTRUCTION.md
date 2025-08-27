## Validation (Logs & Checks)

### A. DaemonSet: перевірка, що curl запускається кожні 5 секунд
```bash
# Поди DaemonSet на всіх нодах
kubectl -n mateapp get pods -l app=todoapp-daemon -o wide

# Візьми ім’я будь-якого пода з попереднього виводу, напр.:
POD_DS=$(kubectl -n mateapp get pods -l app=todoapp-daemon -o jsonpath='{.items[0].metadata.name}')

# Подивись останні логи (повинні з’являтись записи приблизно кожні 5 сек)
kubectl -n mateapp logs "$POD_DS" --tail=100

# За потреби — stream логів у реальному часі:
kubectl -n mateapp logs -f "$POD_DS"

## Validation – CronJob (кожні 4 хвилини)

### 1) Перевірити наявність CronJob і Job-ів
```bash
kubectl -n mateapp get cronjob
kubectl -n mateapp get jobs --sort-by=.metadata.creationTimestamp

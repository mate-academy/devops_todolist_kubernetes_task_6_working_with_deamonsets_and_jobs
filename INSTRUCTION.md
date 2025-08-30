# Инструкции по развертыванию DaemonSet и CronJob для TodoApp

## Описание

Данный репозиторий содержит Kubernetes манифесты для мониторинга Django TodoApp приложения:

- **DaemonSet** (`daemonset.yml`) - запускает мониторинг на всех узлах кластера
- **CronJob** (`cronjob.yml`) - выполняет health check каждые 4 минуты

## Предварительные требования

1. Работающий Kubernetes кластер (версия 1.19+)
2. Установленный `kubectl` с настроенным доступом к кластеру
3. Развернутое TodoApp приложение с ClusterIP сервисом `todoapp-service` на порту 8080
4. Namespace `mateapp` должен существовать в кластере

## Развертывание

### Шаг 1: Создание namespace (если не существует)

```bash
kubectl create namespace mateapp
```

### Шаг 2: Проверка существования TodoApp сервиса

Убедитесь, что сервис TodoApp развернут и доступен:

```bash
kubectl get service todoapp-service -n mateapp
```

Если сервиса нет, создайте его:

```bash
kubectl create service clusterip todoapp-service --tcp=8080:8080 -n mateapp
```

### Шаг 3: Развертывание DaemonSet

Применить манифест DaemonSet:

```bash
kubectl apply -f daemonset.yml
```

Проверить статус развертывания:

```bash
kubectl get daemonset -n mateapp
kubectl get pods -n mateapp -l app=todoapp-monitor
```

### Шаг 4: Развертывание CronJob

Применить манифест CronJob:

```bash
kubectl apply -f cronjob.yml
```

Проверить статус CronJob:

```bash
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
```

## Валидация решения

### 1. Проверка DaemonSet

#### Проверка статуса DaemonSet:

```bash
kubectl get daemonset todoapp-monitor-daemonset -n mateapp -o wide
```

#### Проверка подов DaemonSet:

```bash
kubectl get pods -n mateapp -l app=todoapp-monitor --show-labels
```

#### Просмотр логов DaemonSet (должны показывать curl запросы каждые 5 секунд):

```bash
# Получить имя пода
POD_NAME=$(kubectl get pods -n mateapp -l app=todoapp-monitor -o jsonpath='{.items[0].metadata.name}')

# Просмотр логов
kubectl logs -f $POD_NAME -n mateapp

# Или просмотр логов всех подов DaemonSet
kubectl logs -n mateapp -l app=todoapp-monitor --tail=50
```

**Ожидаемые логи DaemonSet:**

```
2024-01-15 10:00:01: Making curl request to todoapp service
HTTP Status: 200, Time: 0.045s
2024-01-15 10:00:06: Making curl request to todoapp service
HTTP Status: 200, Time: 0.032s
```

### 2. Проверка CronJob

#### Проверка статуса CronJob:

```bash
kubectl get cronjob todoapp-health-check -n mateapp -o wide
```

#### Проверка выполненных Job'ов:

```bash
kubectl get jobs -n mateapp -l app=todoapp-health-check
```

#### Просмотр логов последнего выполнения CronJob:

```bash
# Получить последний Job
LAST_JOB=$(kubectl get jobs -n mateapp -l app=todoapp-health-check --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-1:].metadata.name}')

# Получить под этого Job'а
JOB_POD=$(kubectl get pods -n mateapp -l job-name=$LAST_JOB -o jsonpath='{.items[0].metadata.name}')

# Просмотр логов
kubectl logs $JOB_POD -n mateapp
```

**Ожидаемые логи CronJob:**

```
2024-01-15 10:04:01: Starting health check for todoapp
Target URL: http://todoapp-service:8080/api/health
* Connected to todoapp-service (10.96.x.x) port 8080
> GET /api/health HTTP/1.1
< HTTP/1.1 200 OK
Health OK
2024-01-15 10:04:01: Health check PASSED - Service is healthy
```

### 3. Проверка ресурсов

#### Проверка использования ресурсов DaemonSet:

```bash
kubectl top pods -n mateapp -l app=todoapp-monitor
```

#### Проверка использования ресурсов CronJob:

```bash
kubectl top pods -n mateapp -l app=todoapp-health-check
```

### 4. Мониторинг в реальном времени

#### Отслеживание создания новых Job'ов:

```bash
kubectl get jobs -n mateapp -w
```

#### Отслеживание подов:

```bash
kubectl get pods -n mateapp -w
```

## Дополнительные команды для отладки

### Описание ресурсов:

```bash
kubectl describe daemonset todoapp-monitor-daemonset -n mateapp
kubectl describe cronjob todoapp-health-check -n mateapp
```

### Проверка событий:

```bash
kubectl get events -n mateapp --sort-by=.metadata.creationTimestamp
```

### Проверка сетевой связности:

```bash
# Запустить временный под для тестирования
kubectl run test-pod --image=busyboxplus:curl -n mateapp --rm -it -- /bin/sh

# Внутри пода выполнить:
curl http://todoapp-service:8080/api/health
```

## Очистка ресурсов

Для удаления созданных ресурсов:

```bash
kubectl delete -f daemonset.yml
kubectl delete -f cronjob.yml

# Удалить все связанные Job'ы
kubectl delete jobs -n mateapp -l app=todoapp-health-check
```

## Устранение неполадок

### Если DaemonSet не запускается:

1. Проверьте наличие узлов: `kubectl get nodes`
2. Проверьте tolerations для master узлов
3. Проверьте ресурсы узлов: `kubectl describe nodes`

### Если CronJob не выполняется:

1. Проверьте корректность cron выражения
2. Проверьте часовой пояс кластера
3. Проверьте доступность сервиса: `kubectl get svc -n mateapp`

### Если curl запросы не проходят:

1. Проверьте DNS резолюцию: `nslookup todoapp-service.mateapp.svc.cluster.local`
2. Проверьте сетевые политики
3. Проверьте статус TodoApp: `kubectl get pods -n mateapp`

## Технические детали

### DaemonSet характеристики:

- **Образ**: `busyboxplus:curl`
- **Интервал**: 5 секунд
- **CPU requests/limits**: 10m/50m
- **Memory requests/limits**: 32Mi/64Mi
- **Endpoint**: `http://todoapp-service:8080/`

### CronJob характеристики:

- **Образ**: `busyboxplus:curl`
- **Расписание**: каждые 4 минуты (`*/4 * * * *`)
- **CPU requests/limits**: 5m/20m
- **Memory requests/limits**: 16Mi/32Mi
- **Endpoint**: `http://todoapp-service:8080/api/health`
- **История**: 10 успешных, 5 неудачных выполнений
- **Concurrency Policy**: Allow

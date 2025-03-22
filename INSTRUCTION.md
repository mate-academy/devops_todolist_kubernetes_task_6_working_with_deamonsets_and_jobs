# Інструкція з розгортання DaemonSet та CronJob для TodoApp

## Передумови

*   Встановлений та налаштований Kubernetes кластер.
*   Встановлений `kubectl`.
*   Створений namespace `mateapp`.

## Розгортання

1.  **Створення namespace (якщо ще не створено):**

    ```
    kubectl create namespace mateapp
    ```

2.  **Розгортання DaemonSet:**

    ```
    kubectl apply -f daemonset.yml
    ```

3.  **Розгортання CronJob:**

    ```
    kubectl apply -f cronjob.yml
    ```

## Валідація

1.  **Перевірка DaemonSet:**

    *   Перевірте, чи всі Pod'и DaemonSet знаходяться в стані `Running`:

        ```
        kubectl get pods -n mateapp -l app=todoapp-daemonset
        ```

    *   Перегляньте логи одного з Pod'ів DaemonSet, щоб переконатися, що `curl` запити виконуються кожні 5 секунд:

        ```
        kubectl logs -n mateapp <назва-pod>
        ```

        Замініть `<назва-pod>` на фактичну назву одного з Pod'ів DaemonSet.

2.  **Перевірка CronJob:**

    *   Перевірте, чи CronJob успішно створює Job'и:

        ```
        kubectl get jobs -n mateapp -l cronjob.kubernetes.io/name=todoapp-cronjob
        ```

    *   Перегляньте логи одного з Job'ів CronJob, щоб переконатися, що `curl` запити до `/api/health` виконуються кожні 4 хвилини:

        ```
        kubectl logs -n mateapp <назва-job>
        ```

        Замініть `<назва-job>` на фактичну назву одного з Job'ів, створених CronJob.

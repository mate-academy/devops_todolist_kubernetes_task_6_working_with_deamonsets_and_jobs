## Deployment

To deploy app you need to go to .infrastructure folder and enter following commands:

```bash
kubectl apply -f daemonset.yml
```
```bash
kubectl apply -f cronjob.yml
```

## Validation

To validate you need:

1. Get all **jobs** in **namespace**:

    ```sh
    kubectl get jobs -n mateapp
    ```

2. Check logs for the last **job**:

    ```sh
    kubectl logs job/health-check-cronjob-<your_job_hash> -n mateapp
    ```

3. Get all **pods** to find your daemonset pod:

    ```sh
    kubectl get pods -n mateapp
    ```

4. Check logs for your **daemonset** pod:

    ```sh
    kubectl logs <your_daemonset_pod_name> -n mateapp
    ```
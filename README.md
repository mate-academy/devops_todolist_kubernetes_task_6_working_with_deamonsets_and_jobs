# Deployment Guide
1. **Apply Namespace Configuration**
    ```sh
    kubectl apply -f namespace.yml
    ```

2. **Apply Deployment Configuration**
    ```sh
    kubectl apply -f deployment.yml
    ```

3. **Apply Horizontal Pod Autoscaler (HPA) Configuration**
    ```sh
    kubectl apply -f hpa.yml
    ```

4. **Apply BusyBox Configuration**
    ```sh
    kubectl apply -f busybox.yml
    ```

5. **Apply ClusterIP Service Configuration**
    ```sh
    kubectl apply -f clusterIp.yml
    ```

6. **Apply NodePort Service Configuration**
    ```sh
    kubectl apply -f nodeport.yml
    ```

7. **Apply DaemonSet Configuration**
    ```sh
    kubectl apply -f daemonset.yml
    ```

8. **Apply CronJob Configuration**
    ```sh
    kubectl apply -f cronjob.yml
    ```

## Verification

1. **Check Pods**
    ```sh
    kubectl get pods -n mateapp
    ```

2. **Check DaemonSet**
    ```sh
    kubectl get daemonset -n mateapp
    ```

3. **Check CronJobs**
    ```sh
    kubectl get cronjobs -n mateapp
    ```

4. **Check Logs**
    ```sh
    kubectl logs <name_of_pod> -n mateapp
    ```

Replace `<name_of_pod>` with the actual name of the pod you want to check logs for.

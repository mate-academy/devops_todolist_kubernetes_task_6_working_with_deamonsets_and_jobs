# INSTRUCTION.md

## How to Deploy DaemonSet and CronJob

### Prerequisites
- Ensure you have access to a Kubernetes cluster.
- `kubectl` is installed and configured to access your cluster.

### 1. Create the Namespace
```
kubectl create namespace mateapp
```

### 2. Deploy DaemonSet
```
kubectl apply -f daemonset.yml -n mateapp
```

### 3. Deploy CronJob
```
kubectl apply -f cronjob.yml -n mateapp
```

## Validation

### 1. Check DaemonSet Logs
- List pods created by the DaemonSet:
  ```
  kubectl get pods -n mateapp -l app=todoapp
  ```
- View logs for a DaemonSet pod:
  ```
  kubectl logs <daemonset-pod-name> -n mateapp
  ```

  Replace `<daemonset-pod-name>` with the actual pod name.


### 2. How to Access the busybox Container
- To open a shell inside the busybox container of a DaemonSet pod, run:
  ```
  kubectl exec -it <daemonset-pod-name> -n mateapp -- sh
  ```
  Replace `<daemonset-pod-name>` with the actual pod name.
  This allows you to run commands interactively inside the busybox container.

### 3. Check CronJob Logs
- List jobs created by the CronJob:
  ```
  kubectl get jobs -n mateapp
  ```
- List pods created by the CronJob:
  ```
  kubectl get pods -n mateapp -l job-name=<cronjob-job-name>
  ```
  Replace `<cronjob-job-name>` with the actual job name.
- View logs for a CronJob pod:
  ```
  kubectl logs <cronjob-pod-name> -n mateapp
  ```
  Replace `<cronjob-pod-name>` with the actual pod name.

### 4. Additional Validation
- Ensure the DaemonSet and CronJob are running as expected and logs show successful `curl` requests to the todoapp service endpoints.

---

For any issues, check the status of resources:
```
kubectl get all -n mateapp
```
# Deployment and Validation Instructions

This document provides instructions on how to deploy and validate the `DaemonSet` and `CronJob` in the `mateapp` namespace.

## Prerequisites
- A running Kubernetes cluster.
- The `mateapp` namespace must exist. (If not, run `kubectl create namespace mateapp`).
- The `todoapp` and its service `mateapp-service` must be running in the `mateapp` namespace.

Set your current context to the `mateapp` namespace for easier command execution:
```bash
kubectl config set-context --current --namespace=mateapp
```

## Deployment

1.  **Deploy the DaemonSet**:
    ```bash
    kubectl apply -f .infrastructure/daemonset.yml
    ```

2.  **Deploy the CronJob**:
    ```bash
    kubectl apply -f .infrastructure/cronjob.yml
    ```

## Validation

### 1. Validate DaemonSet
The DaemonSet is designed to run on every node and monitor the service every 5 seconds.

- **Check status**:
  ```bash
  kubectl get daemonset mateapp-daemonset
  ```
- **Check logs**:
  Identify the pod name:
  ```bash
  kubectl get pods -l app=mateapp
  ```
  View the logs (you should see successful curl responses):
  ```bash
  kubectl logs -l app=mateapp
  ```

### 2. Validate CronJob
The CronJob runs every 4 minutes.

- **Check status**:
  ```bash
  kubectl get cronjob mateapp-cronjob
  ```
- **Check history**:
  ```bash
  kubectl describe cronjob mateapp-cronjob
  ```
- **View logs of the most recent run**:
  Identify the latest job pod:
  ```bash
  kubectl get pods | grep mateapp-cronjob
  ```
  View the logs:
  ```bash
  kubectl logs <pod-name-from-above>
  ```

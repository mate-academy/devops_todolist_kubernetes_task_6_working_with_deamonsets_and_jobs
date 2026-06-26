# Deployment and Validation Instructions

## Prerequisites

- A running Kubernetes cluster
- `kubectl` installed and configured
- The ToDo app Pod and Service already deployed in the cluster

## 1. Create the Namespace

```bash
kubectl create namespace mateapp
```

## 2. Deploy the ToDo App Pod and Service

```bash
kubectl apply -f todoapp-pod.yaml
```

## 3. Deploy the DaemonSet and CronJob

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## 4. Validation

### Check DaemonSet pods are running:
```bash
kubectl get pods -n mateapp -l app=todoapp-pinger
```

### Check DaemonSet logs (health-check curl output):
```bash
kubectl logs -n mateapp -l app=todoapp-pinger
```

### Check CronJob status:
```bash
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
```

### Check logs of a CronJob-triggered pod:
```bash
kubectl logs -n mateapp -l app=todoapp-health-checker
```
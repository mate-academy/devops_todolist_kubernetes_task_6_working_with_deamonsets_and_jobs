# 🛠 Deployment Instructions

These steps will help you deploy both `daemonset.yml` and `cronjob.yml` into your Kubernetes cluster.

---

## 🚀 1. Apply the `DaemonSet`

Make sure you are in the project directory and your current Kubernetes context is set correctly.

```bash
kubectl apply -f daemonset.yml
```

✅ Make sure the mateapp namespace exists:

```sh
kubectl create namespace mateapp
```

## ⏰ 2. Apply the CronJob

```sh
kubectl apply -f cronjob.yml
```

## ✅ Validation Instructions
### 🔎 1. Check DaemonSet status
```sh
kubectl get daemonset todoapp-daemonset -n mateapp
```

To verify that it’s running on every node:
```sh
kubectl get pods -n mateapp -l app=todoapp-daemonset -o wide
```

### 📄 2. View logs from DaemonSet

```sh
kubectl logs -l app=todoapp-daemonset -n mateapp
```

This will show logs from all pods that match the DaemonSet label. You can also inspect a specific pod:

```sh
kubectl get pods -n mateapp  # Find a pod name
kubectl logs <pod-name> -n mateapp
```

### 🕒 3. Check CronJob schedule and runs

```sh
kubectl get cronjob todoapp-cronjob -n mateapp
```

To view created jobs:
```sh
kubectl get jobs -n mateapp
```
### 📄 4. View logs from the latest CronJob run

```sh
kubectl get jobs -n mateapp
```

Then check logs:

```sh
kubectl logs <pod-name> -n mateapp
```
## ✅ Expected Output

- DaemonSet logs should show repeated curl requests every 5 seconds to the todoapp ClusterIP.
- CronJob logs should show a curl request to /api/health every 4 minutes.



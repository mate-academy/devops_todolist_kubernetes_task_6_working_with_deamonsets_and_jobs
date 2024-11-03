# Django ToDo list

```bash
kubectl apply -f daemonset.yml
```

```bash
kubectl apply -f cronjob.yml
```

# How to validate the solution. (Logs for the `daemonset` and `cronjob`)

```bash
kubectl get daemonset -n mateapp
```

```bash
kubectl get cronjob -n mateapp
```

```bash
kubectl get pods -n mateapp
```

```bash
kubectl logs <name_of_pod> -n mateapp
```

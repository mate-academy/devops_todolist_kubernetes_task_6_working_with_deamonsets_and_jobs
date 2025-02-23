#### Apply manifests: 
```bash
kubectl apply -f daemonset.yml
```
```bash
kubectl apply -f cronjob.yml
```

 ####  Logs for the `daemonset` and `cronjob`
```bash
kubectl get pods -n mateapp
kubectl get daemonset -n mateapp
kubectl get cronjobs -o wide
```
```bash
kubectl logs <name_of_pod>  -n mateapp
```
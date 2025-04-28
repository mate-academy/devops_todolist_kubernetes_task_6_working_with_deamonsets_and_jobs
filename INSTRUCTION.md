To test task run:
```bash
cd .infrastucture
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml -n todoapp
kubectl apply -f clusterIp.yml -n todoapp
kubectl apply -f daemonset.yml -n todoapp
kubectl apply -f cronjob.yml -n todoapp
```
To see logs for daemonset
1. ```bash 
   kubectl get pods -n todoapp
   ```
2. Get pod id for daemonset and run
```bash
kubectl logs <pod_id>
```

To see logs from cronjob
1. ```bash
    kubectl get pods -n todoapp
    ```
2.Get pod id with pattern `todoapp-cronjob`
```bash
kubectl logs <pod_id>
```

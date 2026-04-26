* how to deploy:
```bash
    kubectl create ns mateapp
    kubectl apply -f .infrastructure/daemonset.yml 
    kubectl apply -f .infrastructure/cronjob.yml 
```
* how to check cron:
```bash
    kubectl logs cronfortodoapp-<pod-hash> -n mateapp
    kubectl get jobs -n mateapp
    kubectl describe cronjob cronfortodoapp -n mateapp
    
```
* how to check daemonset:
```bash
    kubectl logs daemonset-<pod-hash> -n mateapp

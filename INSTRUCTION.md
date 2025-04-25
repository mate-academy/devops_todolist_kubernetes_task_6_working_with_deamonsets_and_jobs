# DeamonSet and CronJob in Kubernetes

These instructions describe how to deploy the app to a Kubernetes cluster using the provided manifests

## Prerequisites

- Kubernetes cluster created and running

## Attached files

- daemonset.yml
- cronjob.yml
- INSTRUCTION.md

## Deployment

- Create the namespace
```bash
    kubectl create namespace mateapp
```

- Deploy daemonset.yml and cronjob.yml
```bash
    cd .infrastructure
    kubectl apply -f daemonset.yml
    kubectl apply -f cronjob.yml
```

## Instructions on how to validate the solution

- 
```bash
    kubectl get daemonset todoapp-daemonset -n mateapp
```

- Make sure it is running on all nodes
```bash
    kubectl get pods -n mateapp -l app=todoapp-daemonset -o wide
```

- 
```bash
kubectl logs -l app=todoapp-daemonset -n mateapp
```

- 
```bash
kubectl logs <name> -n mateapp
```
- 
```bash
kubectl get cronjob todoapp-cronjob -n mateapp
```

- 
```bash
kubectl get jobs -n mateapp
```

- 
```bash
kubectl get jobs -n mateapp
```

- Check logs:
```bash
kubectl logs <name> -n mateapp

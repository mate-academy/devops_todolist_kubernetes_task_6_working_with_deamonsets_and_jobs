1. Recreate and check all feature from previous task

```
kubectl apply -f .infrastructure/namespace.yml
kubectl config set-context --current --namespace=mateapp
kubectl apply -f .infrastructure/deployment.yml
kubectl get deployments -o wide
kubectl apply -f .infrastructure/hpa.yml
kubectl get hpa -o wide
kubectl apply -f .infrastructure/curl.yml
kubectl get pods -o wide
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl get svc -o wide
```

2. Create and check DeamonSet service

```
kubectl apply -f .infrastructure/deamonSet.yml
kubectl get pods -o wide
```

3. Check logs of created pod

```
kubectl logs <name of deamonSet pod from pods list>
```

4. Schedule your Cron Job and wait for a time executing

```
kubectl apply -f .infrastructure/cronJob.yml
```

5. Check for successfully created CronJob by executing

```
kubectl get cronJobs -o wide
```

6. After waiting? get all pods and check for logs of CronJob

```
kubectl get pods -o wide
kubectl logs <name of completed cronjob from pods list>
```

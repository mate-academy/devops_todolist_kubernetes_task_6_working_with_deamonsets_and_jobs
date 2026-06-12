# Deployment and validation instructions

## Deploy the todoapp service

If the todoapp workload is not already running in the cluster, apply the existing manifests first:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

## Create the namespace for the health-check workloads

```bash
kubectl create namespace mateapp --dry-run=client -o yaml | kubectl apply -f -
```

## Deploy the DaemonSet and CronJob

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## Validate the solution

Check that the new workloads are present:

```bash
kubectl get ds -n mateapp
kubectl get cronjob -n mateapp
kubectl get pods -n mateapp
```

Wait a few minutes for the DaemonSet to start and the CronJob to create its first job, then inspect logs:

```bash
kubectl logs -n mateapp -l app=todoapp-health-daemonset --all-containers=true
kubectl get jobs -n mateapp
kubectl logs -n mateapp job/<job-name>
```

The DaemonSet logs should show repeated curl requests, and the CronJob job logs should show the `/api/health` response from the todoapp service.

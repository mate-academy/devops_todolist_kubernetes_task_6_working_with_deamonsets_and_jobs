## Deploy

1. Create namespace (if it does not exist):

```
kubectl apply -f .infrastructure/namespace.yml
```

2. Apply the todoapp service (ClusterIP) so curl has a target:

```
kubectl apply -f .infrastructure/clusterIp.yml
```

3. Apply daemonset and cronjob:

```
kubectl apply -f .infrastructure/deamonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## Validate

1. Check that resources are created:

```
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

2. Validate daemonset

```
kubectl get pods -n mateapp
```

in the list of pods find deamonset's pod and call command

```
kubectl logs todoapp-deamonset-<id> -n mateapp
```

3. Validate cronjob runs and logs:

```
kubectl get pods -n mateapp
```

in the list of pods find cronjob's pod and call command

```
kubectl logs todoapp-cronjob-<job-id>-<pod-hash> -n mateapp
```


## Apply all manifests

```
kubectl apply -f .infractructure/
```

## How to deploy `daemonset.yml` and `cronjob.yml` to the cluster

```
kubectl apply -f daemonset.yml
```

```
kubectl apply -f cronjob.yml
```

## How to validate the solution

```
kubectl get daemonset -n mateapp
```

```
kubectl get cronjob -n mateapp
```

```
kubectl get pods -n todoapp
```

```
kubectl logs <name_of_pod> -n todoapp
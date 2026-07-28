**How to validate the solution**

To verify that our resources are working correctly, we need to check their logs. Because controllers like DaemonSet and CronJob create Pods with dynamic, randomly generated names, it is best to fetch logs directly through the controller itself. 

We do this by specifying the resource type (e.g., `daemonset` or `cronjob`) followed by its specific name.


```bash
kubectl apply -f daemonset.yml
```

```bash
kubectl apply -f cronjob.yml
```

```bash
kubectl get pods -n mateapp -o wide
```

1. Check the logs of the DaemonSet to ensure it makes a `curl` request every 5 seconds:
`kubectl logs -n mateapp daemonset/todoapp`

1. Check the logs of the CronJob to verify it successfully called the health endpoint:
`kubectl logs -n mateapp cronjob/hello-cronjob`


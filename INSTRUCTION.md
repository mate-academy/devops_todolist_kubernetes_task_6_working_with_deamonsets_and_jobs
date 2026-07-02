`INSTRUCTION.md` should contain the instructions on how to deploy `daemonset.yml` and `cronjob.yml` to the cluster.

`INSTRUCTION.md` should be updated with the instructions on how to validate the solution. (Logs for the `daemonset` and `cronjob` should be present)

1. To deploy the `daemonset.yml` and `cronjob.yml` to the cluster, run the following commands in your terminal:
```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

2. To validate the solution, you can check the logs for the `daemonset` and `cronjob` by running the following commands:
```bash
kubectl logs -l app=my-daemonset -n mateapp
kubectl logs -l app=my-cronjob -n mateapp
```

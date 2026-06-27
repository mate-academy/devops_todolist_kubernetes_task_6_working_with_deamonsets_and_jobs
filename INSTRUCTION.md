# Deploying DaemonSet and CronJob

## 1. Deploy

```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

---

## 2. Validate the DaemonSet

The DaemonSet runs one pod per node, continuously curling the ToDo app's ClusterIP service every 5 seconds.

Check that a pod is running on every node:

```bash
kubectl get pods -n mateapp -l app=busybox -o wide
```

```
NAME            READY   STATUS    RESTARTS   AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
busybox-hbk87   1/1     Running   0          18m   10.244.0.6   desktop-control-plane   <none>           <none>
```

Check the logs of any one of these pods to confirm the curl loop is running:

```bash
kubectl logs <pod-name> -n mateapp
```

```
Example:

Sat Jun 27 08:53:03 UTC 2026
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

    ...

<There will be a HTML page>

    ...

100  3747  100  3747    0     0  18758      0 --:--:-- --:--:-- --:--:-- 37848

```

You should see a timestamp and a curl response repeating roughly every 5 seconds.

---

## 3. Validate the CronJob

The CronJob runs every 4 minutes, spinning up a short-lived pod that curls the app's `/api/health` endpoint and then exits.

Check the CronJob's schedule and last run:

```bash
kubectl get cronjob -n mateapp
```

```
NAME                SCHEDULE      TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
api-health-crojob   */4 * * * *   <none>     False     0        34s             20m
```

List the Jobs it has created:

```bash
kubectl get jobs -n mateapp
```

```
NAME                         STATUS     COMPLETIONS   DURATION   AGE
api-health-crojob-29709176   Complete   1/1           3s         20m
api-health-crojob-29709180   Complete   1/1           3s         16m
api-health-crojob-29709184   Complete   1/1           3s         12m
api-health-crojob-29709188   Complete   1/1           3s         8m56s
api-health-crojob-29709192   Complete   1/1           3s         4m56s
api-health-crojob-29709196   Complete   1/1           3s         56s
```

Find the pod from the most recent Job and check its logs:

```bash
kubectl get pods -n mateapp -l job-name=<job-name>
kubectl logs <pod-name> -n mateapp
```

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0    222      0 --:--:-- --:--:-- --:--:--   450
Health OK
```

A successful run shows the health check's response and the pod's status as `Completed`.
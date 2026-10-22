\# Instructions



\## Deployment

To deploy the DaemonSet and CronJob to your cluster, run the following commands from the root directory:

```bash

kubectl apply -f .infrastructure/daemonset.yml

kubectl apply -f .infrastructure/cronjob.yml

```



\## Validation



\### 1. Verify DaemonSet

Check the status of the DaemonSet and its pods:

```bash

kubectl get daemonset -n mateapp

kubectl get pods -n mateapp -l app=todoapp-curl

```

To view the logs and confirm it executes every 5 seconds:

```bash

kubectl logs -n mateapp -l app=todoapp-curl --tail=20

```



\### 2. Verify CronJob

Check the status of the CronJob:

```bash

kubectl get cronjob -n mateapp

```

Once a job triggers (every 4 minutes), you can check the generated pods and view their logs:

```bash

kubectl get pods -n mateapp

\\# View logs for a specific cronjob pod

kubectl logs -n mateapp <cronjob-pod-name>

```


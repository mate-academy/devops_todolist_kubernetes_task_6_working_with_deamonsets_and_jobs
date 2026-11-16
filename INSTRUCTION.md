
# DaemonSet and CronJob: Django ToDo App

## Prerequisites

- A running Kubernetes cluster.
- The Django ToDo application deployed in the `mateapp` namespace.
- A ClusterIP Service named `todoapp` on port 80.
- The `busyboxplus:curl` container image.

Run all commands from the repository root.

Check the existing resources:

```bash
kubectl get namespace mateapp
kubectl get pods -n mateapp
kubectl get svc todoapp -n mateapp
```

### Prepare the container image

The manifests use the required image `busyboxplus:curl`.

For the local Docker Desktop Kubernetes cluster, prepare the image with the following commands:

```bash
docker pull ikulyk404/busyboxplus:curl
docker tag ikulyk404/busyboxplus:curl busyboxplus:curl
```

The local image is available to Kubernetes in the Docker Desktop cluster used for validation. The default image pull policy for this tag is `IfNotPresent`.

## 1. Deploy DaemonSet

Apply the manifest:

```bash
kubectl apply -f .infrastructure/daemonset.yml
```

Check the DaemonSet status:

```bash
kubectl rollout status daemonset/todolist -n mateapp
kubectl get daemonset todolist -n mateapp
```

Check the Pods:

```bash
kubectl get pods -n mateapp -l app=todolist -o wide
```

The DaemonSet creates one Pod on each eligible node. Each container uses the `busyboxplus:curl` image, has CPU and memory requests and limits, and sends an HTTP request to the ToDo ClusterIP Service with a five-second pause between requests.

The Service URL is:

```text
http://todoapp:80
```

### Validate DaemonSet

Check the logs:

```bash
kubectl logs -n mateapp daemonset/todolist --timestamps=true --tail=30
```

Follow the logs to observe repeated requests:

```bash
kubectl logs -f daemonset/todolist -n mateapp --timestamps=true
```

Press Ctrl+C to stop following the logs.

### Actual DaemonSet validation results

The following output was captured on November 16, 2026.

Command:

```bash
kubectl get daemonset todolist -n mateapp
```

Output:

```text
NAME       DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
todolist   1         1         1       1            1           <none>          27m
```

Command:

```bash
kubectl logs -n mateapp daemonset/todolist --timestamps=true --tail=30
```

Actual log excerpt from the ToDo application HTML response. The CSRF token is omitted.

```text
2026-11-16T13:12:53.301537841Z   <div class="row">
2026-11-16T13:12:53.301538591Z     <div class="three columns value-prop"></div>
2026-11-16T13:12:53.301539341Z     <div class="six columns">
2026-11-16T13:12:53.301540050Z       <form action="/todolist/new/" method=post>
2026-11-16T13:12:53.301545133Z       <input type="text" name="description" class="u-full-width" placeholder="Enter your todo" maxlength="128" required id="id_description">
2026-11-16T13:12:53.301549925Z           <dt><input type="submit" class="button button-primary" value="Start one now">
2026-11-16T13:12:53.301550758Z         </dl>
2026-11-16T13:12:53.301551425Z       </form>
2026-11-16T13:12:53.301552050Z     </div>
2026-11-16T13:12:53.301552716Z   </div>
2026-11-16T13:12:53.301553341Z </section>
2026-11-16T13:12:53.301558758Z </body>
2026-11-16T13:12:53.301567675Z </html>
```

Result: the DaemonSet has one desired, ready and available Pod. The captured logs contain the ToDo application's HTML response, confirming that the Pod can reach the application through the ClusterIP Service.

## 2. Deploy CronJob

Apply the manifest:

```bash
kubectl apply -f .infrastructure/cronjob.yml
```

Check the CronJob:

```bash
kubectl get cronjobs -n mateapp
kubectl describe cronjob todolist-cronjob -n mateapp
```

The CronJob configuration:

- Namespace: `mateapp`.
- Container image: `busyboxplus:curl`.
- Schedule: `*/4 * * * *` (every 4 minutes).
- Endpoint: `http://todoapp:80/api/health`.
- CPU and memory requests and limits are defined.
- Successful Jobs history limit: 10.
- Failed Jobs history limit: 5.
- Concurrency policy: `Allow`.
- Restart policy: `OnFailure`.

The CronJob executes the following command:

```bash
date
curl -fsS http://todoapp:80/api/health
```

The curl command exits with a nonzero status on connection failures or HTTP 4xx and 5xx errors.

### Validate CronJob

Wait for the next scheduled execution and check the Jobs:

```bash
kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp
```

Select a completed scheduled Job whose name starts with `todolist-cronjob-`.

During validation, the following Job was completed successfully:

```bash
kubectl get job todolist-cronjob-29913908 -n mateapp
```

Get its logs:

```bash
kubectl logs job/todolist-cronjob-29913908 -n mateapp --timestamps=true
```

For future validation, replace the Job name with the name of the latest completed scheduled Job.

### Actual CronJob validation results

The following output was captured on November 16, 2026.

Job status:

```text
NAME                        STATUS     COMPLETIONS   DURATION   AGE
todolist-cronjob-29913908   Complete   1/1           2s         64s
```

Actual logs:

```text
2026-11-16T13:08:00.715575095Z Mon Nov 16 13:08:00 UTC 2026
2026-11-16T13:08:00.739234803Z Health OK
```

Result: the scheduled Job completed successfully in two seconds. The ToDo application returned `Health OK`, confirming that the CronJob successfully accessed the health endpoint through the ClusterIP Service.

## 3. Validation summary

Both workloads were successfully deployed and validated.

- The DaemonSet Pod is ready and available.
- The DaemonSet logs contain the ToDo application's HTML response.
- The scheduled CronJob completed successfully.
- The CronJob logs contain the `Health OK` response.
- Both workloads belong to the `mateapp` namespace.
- Both manifests use `busyboxplus:curl`.
- Both manifests define CPU and memory requests and limits.

## 4. Cleanup (optional)

To delete the DaemonSet and CronJob:

```bash
kubectl delete -f .infrastructure/daemonset.yml
kubectl delete -f .infrastructure/cronjob.yml
```

These commands remove the DaemonSet and CronJob without removing the existing ToDo application or its ClusterIP Service.

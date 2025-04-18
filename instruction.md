# Deployment Instructions

## Prerequisites

- Kubernetes cluster up and running.
- `kubectl` installed and configured to interact with the cluster.
- Proper permissions to deploy resources to the cluster namespace.

---

## Deployment Steps

### 1. Deploy `daemonset.yml`

1. Apply the `daemonset.yml` file to the cluster using the following command:
   ```bash
   kubectl apply -f daemonset.yml
   ```

2. Verify the DaemonSet is successfully deployed:
   ```bash
   kubectl get daemonset
   ```
   You should see an entry for the DaemonSet with the expected number of pods running.

---

### 2. Deploy `cronjob.yml`

1. Apply the `cronjob.yml` file to the cluster using the following command:
   ```bash
   kubectl apply -f cronjob.yml
   ```

2. Verify the CronJob is successfully deployed:
   ```bash
   kubectl get cronjob
   ```
   You should see an entry for the CronJob. The schedule should also match the configuration in the `cronjob.yml` file.

---

## Validation Steps

### 1. Validate Logs for DaemonSet

To ensure the DaemonSet is running properly, check the logs of one of its pods. Use the following commands:

1. First, list all pods created by the DaemonSet:
   ```bash
   kubectl get pods -l app=<daemonset-label>
   ```
   Replace `<daemonset-label>` with the label used in the `daemonset.yml`.

2. Fetch logs from one of the DaemonSet pods:
   ```bash
   kubectl logs <pod-name>
   ```
   Replace `<pod-name>` with the name of one of the DaemonSet pods. Confirm the expected logs are present.

---

### 2. Validate Logs for CronJob

To verify that the CronJob runs successfully:

1. Check the pods created by the CronJob:
   ```bash
   kubectl get pods -l job-name=<cronjob-name>
   ```
   Replace `<cronjob-name>` with the name specified in the `cronjob.yml`.

2. Fetch logs from the latest CronJob pod:
   ```bash
   kubectl logs <pod-name>
   ```
   Replace `<pod-name>` with the name of a pod created by the CronJob. Confirm the expected logs are present.

---

### 3. Additional Validation

- Confirm resource configurations and statuses using `kubectl describe`:
    - For DaemonSet:
      ```bash
      kubectl describe daemonset <daemonset-name>
      ```
    - For CronJob:
      ```bash
      kubectl describe cronjob <cronjob-name>
      ```
- Ensure both resources are functioning as intended (e.g., pods are running, and jobs are scheduled and completed).

---

## Cleanup (Optional)

To delete the deployed resources, use the following commands:

1. Delete the DaemonSet:
   ```bash
   kubectl delete -f daemonset.yml
   ```
2. Delete the CronJob:
   ```bash
   kubectl delete -f cronjob.yml
   ```
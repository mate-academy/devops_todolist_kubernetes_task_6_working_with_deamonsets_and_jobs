# Deployment Instructions

This document provides the instructions to deploy a `DaemonSet` and a `CronJob` to your Kubernetes cluster. Additionally, validation steps are outlined to ensure the solution is working correctly.

---

## Prerequisites

Make sure the following prerequisites are met before proceeding:

- You have access to a Kubernetes cluster.
- The `kubectl` CLI tool is installed and authenticated to the Kubernetes cluster.
- The `daemonset.yml` and `cronjob.yml` files are present in your working directory.

---

## Deployment Steps

1. **Apply the `DaemonSet` to the Cluster:**

   Use the `kubectl` command to deploy the `DaemonSet`:

   ```bash
   kubectl apply -f daemonset.yml
   ```

2. **Apply the `CronJob` to the Cluster:**

   Similarly, use the following command to deploy the `CronJob`:

   ```bash
   kubectl apply -f cronjob.yml
   ```

3. **Verify Resource Creation:**

   After deploying the resources, verify that the `DaemonSet` and `CronJob` were successfully created:

   ```bash
   kubectl get daemonsets
   kubectl get cronjobs
   ```

   Ensure the resources appear in the list and are correctly configured.

---

## Validation Steps

### 1. **Check Logs for the `DaemonSet`:**

To validate that the `DaemonSet` pods are running correctly, check the logs for one of the pods:

   ```bash
   kubectl get pods -l app=<daemonset_label>
   ```

Replace `<daemonset_label>` with the appropriate label defined in your `DaemonSet`. Then, check the logs of a specific pod by replacing `<pod_name>`:

   ```bash
   kubectl logs <pod_name>
   ```

Confirm that the logs indicate the `DaemonSet` is functioning as expected.

### 2. **Check Logs for the `CronJob`:**

For the `CronJob`, find the pods created by its scheduled tasks:

   ```bash
   kubectl get pods -l job-name=<cronjob_name>
   ```

Replace `<cronjob_name>` with the name of the `CronJob`. Then, retrieve the logs of the corresponding pod:

   ```bash
   kubectl logs <pod_name>
   ```

Validate that the logs demonstrate the `CronJob` is executing and behaving as intended.

### 3. **Validate Resource Status:**

To check the status of the `DaemonSet`:

   ```bash
   kubectl describe daemonset <daemonset_name>
   ```

Validate that all desired pods are running on the nodes.

To check the schedule and execution status of the `CronJob`:

   ```bash
   kubectl describe cronjob <cronjob_name>
   ```

Ensure the schedule and execution history are as per expectations.

---

## Cleanup

To remove the deployed resources from your cluster:

1. Delete the `DaemonSet`:

   ```bash
   kubectl delete -f daemonset.yml
   ```

2. Delete the `CronJob`:

   ```bash
   kubectl delete -f cronjob.yml
   ```

---

Following these steps ensures proper deployment, validation, and cleanup for the `DaemonSet` and `CronJob`.
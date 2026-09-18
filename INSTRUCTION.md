# Comprehensive Instructions: Deployment & Validation of DaemonSet and CronJob

This repository contains Kubernetes manifests for deploying a continuous monitoring **DaemonSet** and a scheduled **CronJob** within the `mateapp` namespace, interacting with the internal `todoapp-service`.

---

## 🏗️ Architecture & Requirements Overview

1. **DaemonSet (`daemonset.yml`)**:
   * **Purpose**: Ensures a monitoring/utility pod runs on every worker node in the cluster.
   * **Container**: `busyboxplus:curl`
   * **Resources**: Requests & Limits set (CPU: `100m`, Memory: `64Mi`).
   * **Action**: Executes an infinite loop running `curl` against the internal cluster IP service (`http://todoapp-service.mateapp.svc.cluster.local`) every **5 seconds**.

2. **CronJob (`cronjob.yml`)**:
   * **Purpose**: Periodically triggers a task on a schedule.
   * **Schedule**: Every 4 minutes (`*/4 * * * *`).
   * **Concurrency Policy**: `Allow` (allows multiple runs to overlap if needed).
   * **History Limits**: Retains up to **10 successful** runs and **5 failed** runs.
   * **Action**: Calls the health check endpoint (`/api/health`) of the todoapp service.

---

## 📋 Prerequisites

* A running Kubernetes cluster (e.g., `kind`, Minikube, or a managed cloud cluster).
* `kubectl` CLI tool installed and configured.
* Target namespace created in the cluster.

---

## 🚀 Step-1: Namespace Setup

If the `mateapp` namespace does not exist yet, create it:
```bash
kubectl create namespace mateapp
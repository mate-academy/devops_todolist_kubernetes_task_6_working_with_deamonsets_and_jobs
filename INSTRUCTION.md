## Deployment Steps

### Step 1: Create Namespaces

Create the required namespaces for the application:

```bash
# Create the todoapp namespace (for deployment and service)
kubectl apply -f .infrastructure/namespace.yml

# Create the mateapp namespace (for daemonset and cronjob)
kubectl apply -f .infrastructure/namespace-mateapp.yml
```

Verify namespaces were created:
```bash
kubectl get namespaces
```

Expected output should include both `todoapp` and `mateapp` namespaces.

---

### Step 2: Deploy the TodoApp Application

Deploy the Deployment and ClusterIP Service to the `todoapp` namespace:

```bash
# Deploy the TodoApp Deployment
kubectl apply -f .infrastructure/deployment.yml

# Deploy the ClusterIP Service
kubectl apply -f .infrastructure/clusterIp.yml
```

Verify the deployment and service are created:
```bash
# Check deployments
kubectl get deployments -n todoapp

# Check services
kubectl get services -n todoapp

# Check pods
kubectl get pods -n todoapp
```

Wait for the pod to be in `Running` state:
```bash
kubectl get pods -n todoapp -w
```

---

### Step 3: Deploy the DaemonSet

Deploy the DaemonSet that runs curl daemon on all nodes:

```bash
kubectl apply -f .infrastructure/daemonset.yml
```

Verify the DaemonSet is created:
```bash
kubectl get daemonsets -n mateapp

kubectl get pods -n mateapp -l app=todoapp-curl
```

The number of DaemonSet pods should match the number of worker nodes in your cluster.

---

### Step 4: Deploy the CronJob

Deploy the CronJob that performs health checks every 4 minutes:

```bash
kubectl apply -f .infrastructure/cronjob.yml
```

Verify the CronJob is created:
```bash
kubectl get cronjobs -n mateapp

kubectl describe cronjob todoapp-health-check -n mateapp
```

---

## Validation Steps

### 1. Verify Namespace Creation

```bash
kubectl get namespaces
```

You should see:
- `todoapp` namespace
- `mateapp` namespace

---

### 2. Verify TodoApp Deployment and Service

```bash
# Check deployment status
kubectl get deployment todoapp -n todoapp

# Check service
kubectl get service todoapp-service -n todoapp

# Check pod status
kubectl get pods -n todoapp
```

Ensure all pods are in `Running` state and `Ready` status shows `1/1`.

---

### 3. Verify DaemonSet

```bash
# View DaemonSet details
kubectl get daemonsets -n mateapp

# View all DaemonSet pods
kubectl get pods -n mateapp -l app=todoapp-curl
```

Expected output:
- One pod per node in your cluster
- All pods in `Running` state

#### Check DaemonSet Logs

```bash
# Get logs from a specific DaemonSet pod
kubectl logs -n mateapp -l app=todoapp-curl --tail=20

# Follow logs in real-time
kubectl logs -n mateapp -l app=todoapp-curl -f

# Get logs from a specific pod
kubectl logs -n mateapp <pod-name> --tail=50
```

**Expected Log Output:**
The logs should show continuous curl responses from the TodoApp service:
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    11  100    11    0     0     22      0 --:--:-- --:--:-- --:--:-- --:--:00
OK
```

---

### 4. Verify CronJob

```bash
# View CronJob details
kubectl get cronjobs -n mateapp

# View CronJob description
kubectl describe cronjob todoapp-health-check -n mateapp
```

#### Check CronJob Logs

CronJob creates Job objects that can be viewed with:

```bash
# View all jobs created by the cronjob
kubectl get jobs -n mateapp

# View job history
kubectl get jobs -n mateapp -o wide

# Get logs from a specific CronJob job
kubectl logs -n mateapp -l job-name=<job-name>

# Get the latest CronJob job and view its logs
kubectl logs -n mateapp $(kubectl get pods -n mateapp -l job-name=$(kubectl get jobs -n mateapp -o jsonpath='{.items[-1].metadata.name}') -o jsonpath='{.items[0].metadata.name}')
```

**Expected Log Output:**
The logs should show successful health check requests:
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    11  100    11    0     0   129    129 --:--:-- --:--:-- --:--:-- --:--:00
OK
```

#### Monitor CronJob Execution

To watch CronJob jobs being created and executed:
```bash
# Watch jobs as they are created
kubectl get jobs -n mateapp -w

# View recent jobs
kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp
```

The CronJob runs every 4 minutes (`*/4 * * * *`), so you should see a new job created approximately every 4 minutes.
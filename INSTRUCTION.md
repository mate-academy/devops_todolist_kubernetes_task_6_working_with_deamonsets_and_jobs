# =========================
# PREREQUISITES
# =========================

# Verify cluster access
kubectl cluster-info

# (Optional) Create namespaces if they do not exist
kubectl create namespace todoapp || true
kubectl create namespace mateapp || true


# =========================
# DEPLOYMENT
# =========================

# Deploy DaemonSet
kubectl apply -f daemonset.yml

# Verify DaemonSet creation
kubectl get daemonset -n todoapp

# Verify DaemonSet Pods
kubectl get pods -n todoapp -l app=todoapp


# Deploy CronJob
kubectl apply -f cronjob.yml

# Verify CronJob creation
kubectl get cronjob -n mateapp


# =========================
# VALIDATION
# =========================

# ---- DaemonSet validation ----

# Check DaemonSet Pods status
kubectl get pods -n todoapp -l app=todoapp

# View logs from all DaemonSet Pods
kubectl logs -n todoapp -l app=todoapp --tail=50

# Expected:
# - Logs contain curl output
# - Requests repeat continuously (e.g. every 5 seconds)


# ---- CronJob validation ----

# Check CronJob status
kubectl get cronjob -n mateapp

# List Jobs created by CronJob
kubectl get jobs -n mateapp

# Verify Job history limits
# - successfulJobsHistoryLimit = 10
# - failedJobsHistoryLimit = 5
kubectl get jobs -n mateapp

# Get Pods created by a specific Job
kubectl get pods -n mateapp --selector=job-name=<job-name>

# View logs from a CronJob execution
kubectl logs -n mateapp job/<job-name>

# Expected:
# - Logs show a curl call to http://todoapp/api/health
# - Job runs every 4 minutes
# - Concurrent Jobs are allowed (concurrencyPolicy=Allow)



# CLEANUP (OPTIONAL)
kubectl delete -f daemonset.yml
kubectl delete -f cronjob.yml

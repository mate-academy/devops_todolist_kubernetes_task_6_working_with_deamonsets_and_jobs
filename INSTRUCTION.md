# How to deploy and validate Todo app with DaemonSet and CronJob

## 1. Build and push Docker image (only once)

From the `src` folder:

```bash
cd src

# Build image for Django Todo application
docker build -t ikulyk404/todoapp:3.0.0 .

# Push image to Docker Hub
docker push ikulyk404/todoapp:3.0.0

# Go back to repository root
cd ..

# Create namespace
kubectl apply -f _infrastructure/namespace.yml

# Create ClusterIP service and NodePort (for manual checks in browser)
kubectl apply -f _infrastructure/clusterIp.yml
kubectl apply -f _infrastructure/nodeport.yml

# Create Deployment with the Todo app and HPA
kubectl apply -f _infrastructure/deployment.yml
kubectl apply -f _infrastructure/hpa.yml

# Check that everything is running in the mateapp namespace
kubectl get pods -n mateapp
kubectl get deploy -n mateapp
kubectl get svc -n mateapp
kubectl get hpa -n mateapp

# Deploy DaemonSet and CronJob
kubectl apply -f _infrastructure/daemonset.yml
kubectl apply -f _infrastructure/cronjob.yml

# Verify that the resources were created
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp

# List DaemonSet pods
kubectl get pods -n mateapp -l app=todoapp-daemon

# Watch logs from all DaemonSet pods
kubectl logs -n mateapp -l app=todoapp-daemon -f

# In the logs you should see repeated messages like:

# DaemonSet ping todoapp-service

# HTTP response from /api/health (or error messages if the app is not ready)

# This confirms that the DaemonSet is correctly calling the Todo app through the ClusterIP service.


# CronJob logs and history
# CronJob should run every 4 minutes and call /api/health via the same ClusterIP service.
# Check CronJob and created Jobs
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
kubectl get pods -n mateapp -l job=todoapp-healthcheck

# Replace <job-name> with the real job name from the previous command
kubectl logs job/<job-name> -n mateapp

# Get node external IP
kubectl get nodes -o wide

# To remove all resources created for this task
kubectl delete -f _infrastructure/cronjob.yml
kubectl delete -f _infrastructure/daemonset.yml
kubectl delete -f _infrastructure/hpa.yml
kubectl delete -f _infrastructure/deployment.yml
kubectl delete -f _infrastructure/nodeport.yml
kubectl delete -f _infrastructure/clusterIp.yml
kubectl delete -f _infrastructure/busybox.yml
kubectl delete -f _infrastructure/todoapp-pod.yml
kubectl delete -f _infrastructure/namespace.yml

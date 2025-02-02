# Instructions on how to deploy daemonset.yml and cronjob.yml to the cluster.

# How to deploy the DaemonSet to your cluster:

kubectl apply -f daemonset.yml

# How to verify the DaemonSet is running:
kubectl get daemonset -n todoapp
kubectl get pods -n todoapp

# How to Deploy the CronJob to your cluster:
kubectl apply -f cronjob.yml

# How to Verify the CronJob is created:
kubectl get cronjob -n todoapp

# How to Check the logs of the CronJob pod:
kubectl get jobs -n todoapp
Then, check the logs of the pod associated with the job:

kubectl get pods -n todoapp
kubectl logs <pod-name> -n todoapp


# Validating the Solution:
The DaemonSet should be running the curl command periodically to check the /api/health endpoint of the todoapp service.
# How to Check the logs of the DaemonSet pods:

kubectl logs <daemonset-pod-name> -n todoapp

# How to check CronJob Logs:

The CronJob will execute every 4 minutes and call the /api/health endpoint.

kubectl get jobs -n todoapp
kubectl logs <cronjob-pod-name> -n todoapp




# Instructions for Deploying DaemonSet and CronJob

## Deploying DaemonSet
1. Apply the `daemonset.yml` file:
   ```bash
   kubectl apply -f daemonset.yml
2. Verify that the DaemonSet is running:

   kubectl get daemonsets -n todoapp
3. Check logs for each pod in the DaemonSet:

   kubectl logs -f -n todoapp <pod-name>

## Deploying CronJob
1. Apply the cronjob.yml file:

   kubectl apply -f cronjob.yml
2. Verify that the CronJob is scheduled:

   kubectl get cronjobs -n todoapp
3. Check the logs of the CronJob:

   kubectl get jobs -n todoapp
   kubectl logs -f <job-pod-name> -n todoapp
## Validating the Solution
 - Ensure the DaemonSet is running and periodically making requests to the todoapp-service.
 - Check the CronJob to ensure it runs every 4 minutes and correctly accesses the /api/health endpoint.
## Why this Configuration?
 - DaemonSet: Ensures that a curl request is made from every node in the cluster to the todoapp service, helping to monitor app availability across all nodes.
 - CronJob: Periodically checks the health of the todoapp service every 4 minutes to ensure that the API is responding correctly.
 - Resource Requests and Limits: Ensures that the containers have sufficient resources to run without overloading the nodes, with proper limits to avoid resource contention.
## Accessing the Application
 - Use the todoapp-service (ClusterIP) for internal communication between pods.
 - If you need to access the app externally, use the todoapp-nodeport service.

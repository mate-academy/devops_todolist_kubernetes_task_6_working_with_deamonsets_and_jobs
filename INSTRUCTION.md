📘 INSTRUCTION.md
🔧 Deployment Instructions
Namespace used: mateapp

1. Create the namespace (if not exists):

kubectl create namespace mateapp
2. Deploy the DaemonSet:

kubectl apply -f daemonset.yml
3. Deploy the CronJob:

kubectl apply -f cronjob.yml
✅ Validation Instructions
🔍 Check DaemonSet logs
Get DaemonSet pods:


kubectl get pods -n mateapp -l name=todoapp-curl-daemon
Check logs of any pod:


kubectl logs <daemonset-pod-name> -n mateapp
You should see repeating output of curl requests to http://todoapp:8000.

🔍 Check CronJob logs
List CronJobs:


kubectl get cronjob -n mateapp
Check Job runs:

kubectl get jobs -n mateapp
Check logs of the latest CronJob pod:


kubectl get pods -n mateapp --sort-by=.metadata.creationTimestamp
kubectl logs <latest-cronjob-pod-name> -n mateapp
You should see a one-time call to:

curl http://todoapp:8000/api/health
📤 How to Redeploy
If you make changes to the YAML files:


kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
To delete resources (for clean-up or retry):


kubectl delete -f daemonset.yml
kubectl delete -f cronjob.yml
✅ Done!
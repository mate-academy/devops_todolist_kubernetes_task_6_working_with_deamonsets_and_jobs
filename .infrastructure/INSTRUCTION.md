## Instructions for deploying the ToDo app in a Kubernetes cluster, as well as the daemonset.yml and cronjob.yml files in that cluster.
### Create a namespace in your cluster by running the following command:
```
kubectl apply -f namespace.yml
```

For added convenience, set the default namespace context for 
```
kubectl config set-context --current --namespace=mateapp
```
Create deployment:
```
kubectl apply -f deployment.yml
```
Create clusterIp:
```
kubectl apply -f clusterIp.yml
```
Create NodePort:
```
kubectl apply -f nodeport.yml 
```
Create HPA:
```
kubectl apply -f hpa.yml
```
Create DeamonSet:
```
kubectl apply -f daemonset.yml
```
Create CronJob:
```
kubectl apply -f cronjob.yml 
```
To review the DeamonSet logs, check the username of the user who created the daemonset.yml manifest using the command  ```kubectl get pods```
Check the logs for this pod:
```
kubectl logs <pod-daemonset-name>
```
The logs should show successful HTTP responses from curl commands

To check the CronJob logs, select the name of the job created by the corresponding manifest, and run the command:
```
kubectl logs <pod-cronJob-name>
```
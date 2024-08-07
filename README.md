# Apply new manifests and view logs 

kubectl apply -f .infrastructure/deamonSet.yml

kubectl apply -f .infrastructure/cronjob.yml


# Check workability of manifests

kubectl get pods -o wide

kubectl get cronJobs -o wide

kubectl logs <name of deamonSet pod from pods list>

kubectl logs <name of completed cronjob from pods list>

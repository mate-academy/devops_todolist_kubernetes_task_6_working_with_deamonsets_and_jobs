# Django ToDo list

## Deployment instructions to the cluster
1. Get into the `.infrastructure` directory to apply manifests
2. Use `kubectl apply -f daemonset.yml`
3. Use `kubectl apply -f cronjob.yml`


## Checking daemonset
1. Use `kubectl get node -o wide`. We will need field `INTERNAL-IP` for testing
2. Use `kubectl exec -it -n todoapp busybox -- sh`. 
3. Use ip from the `INTERNAL-IP` field and write `curl http://INTERNAL-IP:30007`.
Your command should look like `curl http://172.18.0.4:30008`

## Checking cronjob
1. Use `kubectl get cronjobs -o wide -n mateapp` to check if cronjob is running
2. Use `kubectl get pods -n mateapp`
3. Use `kubectl logs PodName`. Your command should look like `kubectl logs todoapp-cronjob-28579140-hr4ht -n mateapp`
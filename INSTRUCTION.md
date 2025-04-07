# Django ToDo list
## Applying a new manifest
In the terminal run the following command
1. `cd .\.infrastructure\`
2. `kubectl apply -f daemonset.yml`
3. `kubectl apply -f cronjob.yml`
## How to validate the solution
To check if the resources were successfully created, run the following command
- `kubectl get daemonset -n mateapp`
- `kubectl get cronjob -n mateapp`
## How to get logs
To get more information about the application, run the commands
- `kubectl get pods -n mateapp`
- `kubectl logs <todo-server-(а_DaemonSet)> -n mateapp`
- `kubectl logs <curl-job-(а_CronJob)> -n mateapp`
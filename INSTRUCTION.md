# Django ToDo List
## Deploying manifests to cluster
1.  ```bash
    cd .infrastructure/
    ```
2. ``` bash
   kubectl apply -f daemonset.yml
   ```
3. ```bash
   kubectl apply -f cronjob.yml
   ```
## Validation of the solution
### Daemonset:
```bash
kubectl get daemonset -n mateapp
```
### CronJob:
```bash
kubectl get cronjob -n mateapp
```
## Getting logs
### Daemonset:
Run this command to look up for pods:
```bash
kubectl get pods -n mateapp
```
Needed pod look like todoapp-server-< random_values >
```bash
kubectl logs todoapp-server-<random_values> -n mateapp
```
### CronJob:
Run this command to look up for pods:
```bash
kubectl get pods -n mateapp
```
Needed pod look like health-check-< random_values >
```bash
kubectl logs health-check-<random_values> -n mateapp
```
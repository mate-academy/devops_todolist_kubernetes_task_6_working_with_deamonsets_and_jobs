Run commands from ./infrastructure workdir
### First of all, create this resources:
```
    kubectl apply -f ./namespace.yml
    kubectl apply -f ./clusterIp.yml
    kubectl apply -f ./deployment.yml
```
## daemonset and cronjob:
```angular2html
    kubectl apply -f ./daemonset.yml
    kubectl apply -f ./cronjob.yml
```

## Validating solution:
### To validate daemonset:
```angular2html
kubectl get pods -n mateapp
```
and find pod with "curl-daemon" name prefix. For example: on my node it was pod "curl-daemon-qplgc"

Next, check logs from your pod: ``` kubectl logs -n mateapp curl-daemon-qplgc```

### To validate cronjob:
Wait 4 minutes for first time cronjob "healthcheck" doing job.

Find pod with "healthcheck" prefix. For example: on my node it was pod "healthcheck-29630640-d48kc"
Next, check logs from your pod: ``` kubectl logs -n mateapp healthcheck-29630640-d48kc```
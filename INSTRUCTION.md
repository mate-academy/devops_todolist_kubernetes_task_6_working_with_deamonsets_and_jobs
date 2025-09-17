## Before start verify that all previous k8s manifests are applied

## Verify that namespace mateapp exists. If not, use the command bellow to create this namespace:
```shell
    kubectl create namespace mateapp
```

## To apply daemonset and cronjob manifests run the next commands:
- ```kubectl apply -f daemonset.yml -n mateapp```
- ```kubectl apply -f cronjob.yml -n mateapp```

## Verify that daemonset works correctly:
- run ```kubectl get pods -o wide -n mateapp``` to list all pods
- run ```kubectl logs <daemonset-pod-name> -n mateapp``` to check daemonset pod logs


## Verify that cronjob works correctly:
- run ```kubectl get pods -o wide -n mateapp``` to list all pods
- run ```kubectl logs <cronjob-pod-name> -n mateapp``` to check cronjob pod logs

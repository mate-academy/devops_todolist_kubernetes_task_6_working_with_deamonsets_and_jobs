## How to deploy daemonset.yml and cronjob.yml to the cluster.

``

    kubectl apply -f deamonset.yml
    kubectl apply -f cronJob.yml

``

## How to validate the solution.

``
    kubectl get nodes -o wide
    kubectl exec -it -n mateapp busybox -- sh
    curl http://{ip}:30008
``

``

    kubectl get jobs
    kubectl logs {name}

``
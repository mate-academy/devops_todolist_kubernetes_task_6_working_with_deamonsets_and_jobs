## to apply manifests execute commands:
1. daemonset: `kubectl apply -f .\.infrastructure\daemonset.yml`
2. cronjob: `kubectl apply -f .\.infrastructure\cronjob.yml`

# to check logs
1. Get pod name by `kubectl get pods`
2. Look throw logs by `kubectl logs {pod_name}`
To deploy daemonset.yml and cronjob.yml use commands:

    cd ./.infrastructure
    
    kubectl apply -f daemonset.yml

    kubectl apply -f cronjob.yml

To watch logs use commands:

    To see pod's names use command:

        kubectl get pods

    kubectl logs <daemonset_pod_name>

    kubectl logs <cronjob_pod_name>

1. How to deploy `daemonset.yml` and `cronjob.yml` to the cluster.

    - Run the daemonset from `daemonset.yml` manifest:

        cd .\.infrastructure\ && kubectl apply -f daemonset.yml
    
    - Run the cronjob from `cronjob.yml` manifest:

        cd .\.infrastructure\ && kubectl apply -f cronjob.yml


2. How to validate the solution.

    - Check that DaemonSet pods was created.

        kubectl -n mateapp get pods -l app=busybox-daemonset

    - Check logs for the DaemonSet:

        kubectl -n mateapp logs <pod>

    - Check that CronJob pods was created.

        kubectl -n mateapp get pods -l app=todoapp-cronjob

    - Check logs for the CronJob:

        kubectl -n mateapp logs <pod>



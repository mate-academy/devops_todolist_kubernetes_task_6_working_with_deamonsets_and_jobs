# 1) How to deploy cronjon.yml and daemonset.yml:

    kubectl apply -f daemonset.yml
    kubectl apply -f cronjob.yml

# 2) How to validate the solution:

**DaemonSetLogs:**

Firstly try to get the list our pods:

    kubectl -n mateapp get pods -l app=daemonset-todoapp

And then we can check the logs our pods:

    kubectl -n mateapp logs <name-pod>

**CronJob logs:**

Check list our pods:

    kubectl -n mateapp get pods --selector=job-name=<cronjob-todoapp>


Check logs: 

    kubectl -n mateapp logs <name-pod>

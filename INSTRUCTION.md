1. To deploy daemonset.yml and cronjob.yml to the cluster.

use: 
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml

2. to test them

kubectl get pods -n mateapp -o wide

curl http://<node-ip>:30008

kubectl get jobs -n mateapp --watch
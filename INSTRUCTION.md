1. Instruction on  how to deploy 
  ll
  cd .infrastructure
  kubectl apply -f daemonset.yml
  kubectl get pods -n mateapp -o wide
  kubectl logs (pod`s name) -n mateapp

2. kubectl apply -f cronjob.yml
  kubectl get pods -n mateapp -o wide
  kubectl logs (pod`s name) -n mateapp
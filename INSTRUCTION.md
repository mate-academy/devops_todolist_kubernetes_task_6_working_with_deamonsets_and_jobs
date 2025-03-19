Start the DaemonSet by command:
```
kubectl apply -f .infrastructure/daemonset.yml
```

To check it is working properly, get pods name:
```
kubectl get pods
```

Find the pod with the name starting with 
```
todoapp-daemon-<something>
```

Get logs from this pod:
```
kubectl logs todoapp-daemon-<something>
```
It must show the html code from the main page of our website

----------------------------------------------------------------

Then start the cronjob, that will be executed every 4 minutes and it will check health of our app:
```
kubectl apply -f .infrastructure/cronjob.yml
```

Check that job is registered:
```
kubectl get jobs
```

Get list of all pods:
```
kubectl get pods
```

And check the logs of the cronjob container
```
kubectl logs <cronjob container name>
```

It must be giving us something like this:
```
Thu Mar  6 09:46:01 UTC 2025
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0     20      0 --:--:-- --:--:-- --:--:--    37
```

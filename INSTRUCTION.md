# INSTRUCTION.md

## Deploying DaemonSet and CronJob

To deploy the `daemonset.yml` and `cronjob.yml` to the cluster, run the following commands:

```bash
# Create the namespace
kubectl apply -f .infrastructure/namespace.yml

# Deploy the main application
kubectl apply -f .infrastructure/deployment.yml

# Deploy ClusterIP service for the ToDo app
kubectl apply -f .infrastructure/clusterIp.yml

# Deploy the DaemonSet
kubectl apply -f .infrastructure/daemonset.yml

# Deploy the CronJob
kubectl apply -f .infrastructure/cronjob.yml
```

---

## Checking Pods

To list all pods in the `mateapp` namespace:

```bash
kubectl get pods -n mateapp
```

---

## Viewing Logs

To view logs of a specific pod:

```bash
kubectl logs <pod-name> -n mateapp
```

---

## Example Output

### DaemonSet Pod Logs

```text
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                               Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host todoapp-service.mateapp.svc.cluster.local:80 was resolved.
```

### CronJob Pod Logs

```text
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                               Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host todoapp-service.mateapp.svc.cluster.local:80 was resolved.
* IPv6: (none)
* IPv4: 10.100.113.133
*   Trying 10.100.113.133:80...
* Connected to todoapp-service.mateapp.svc.cluster.local (10.100.113.133) port 80
> GET /api/health HTTP/1.1
> Host: todoapp-service.mateapp.svc.cluster.local
> User-Agent: curl/8.9.0
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Date: Sat, 27 Sep 2025 10:52:02 GMT
< Server: WSGIServer/0.2 CPython/3.8.19
< Content-Type: text/plain
< X-Frame-Options: DENY
< Content-Length: 9
< X-Content-Type-Options: nosniff
< Referrer-Policy: same-origin
< Cross-Origin-Opener-Policy: same-origin
< 
{ [9 bytes data]
100     9  100     9    0     0     15      0 --:--:-- --:--:-- --:--:--    22
* Connection #0 to host todoapp-service.mateapp.svc.cluster.local left intact
Health OK
```

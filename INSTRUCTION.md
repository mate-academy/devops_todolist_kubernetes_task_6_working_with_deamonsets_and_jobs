I used image: ikulyk404/busyboxplus:curl because busyboxplus:curl image doesn't excist.
that's why pods didn't start.
Need run commands:
cd .infrastructure
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml



2. how to validate the solution. (Logs for the daemonset and cronjob should be present)
Need run commands:
kubectl get pods -o wide
Find name pods witch start with my-server-****
kubectl logs my-server-****
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host todoapp-service.todoapp.svc.cluster.local:80 was resolved.
* IPv6: (none)
* IPv4: 10.96.48.201
*   Trying 10.96.48.201:80...
* Connected to todoapp-service.todoapp.svc.cluster.local (10.96.48.201) port 80
> GET /api/ HTTP/1.1
> Host: todoapp-service.todoapp.svc.cluster.local
> User-Agent: curl/8.9.0
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Date: Wed, 24 Jun 2026 09:19:09 GMT
< Server: WSGIServer/0.2 CPython/3.8.19
< Content-Type: application/json
< Vary: Accept, Cookie
< Allow: GET, HEAD, OPTIONS
< X-Frame-Options: DENY
< Content-Length: 219
< X-Content-Type-Options: nosniff
< Referrer-Policy: same-origin
< Cross-Origin-Opener-Policy: same-origin
< 
{ [219 bytes data]
100   219  100   219    0     0    703      0 --:--:-- --:--:-- --:--:--  1052
* Connection #0 to host todoapp-service.todoapp.svc.cluster.local left intact
{"users":"http://todoapp-service.todoapp.svc.cluster.local/api/users/","todolists":"http://todoapp-service.todoapp.svc.cluster.local/api/todolists/","todos":"http://todoapp-service.todoapp.svc.cluster.local/api/todos/"}Request at Wed Jun 24 09:19:15 UTC 2026
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host todoapp-service.todoapp.svc.cluster.local:80 was resolved.
* IPv6: (none)
* IPv4: 10.96.48.201
*   Trying 10.96.48.201:80...
* Connected to todoapp-service.todoapp.svc.cluster.local (10.96.48.201) port 80
> GET /api/ HTTP/1.1
> Host: todoapp-service.todoapp.svc.cluster.local
> User-Agent: curl/8.9.0
> Accept: */*
> 
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Request completely sent off
< HTTP/1.1 200 OK
< Date: Wed, 24 Jun 2026 09:19:16 GMT
< Server: WSGIServer/0.2 CPython/3.8.19
< Content-Type: application/json
< Allow: GET, HEAD, OPTIONS
< X-Frame-Options: DENY
< Content-Length: 219
< X-Content-Type-Options: nosniff
< Referrer-Policy: same-origin
< Cross-Origin-Opener-Policy: same-origin
< 
{ [219 bytes data]
100   219  100   219    0     0    538      0 --:--:-- --:--:-- --:--:--   720
* Connection #0 to host todoapp-service.todoapp.svc.cluster.local left intact

How to validate the solution. (cronjob)
kubectl get cronjobs
kubectl logs my-cronjob-29705032-zpw7j
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host todoapp-service.todoapp.svc.cluster.local:80 was resolved.
* IPv6: (none)
* IPv4: 10.96.48.201
*   Trying 10.96.48.201:80...
* Connected to todoapp-service.todoapp.svc.cluster.local (10.96.48.201) port 80
> GET /api/health HTTP/1.1
> Host: todoapp-service.todoapp.svc.cluster.local
> User-Agent: curl/8.9.0
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Date: Wed, 24 Jun 2026 11:52:01 GMT
< Server: WSGIServer/0.2 CPython/3.8.19
< Content-Type: text/plain
< X-Frame-Options: DENY
< Content-Length: 9
< X-Content-Type-Options: nosniff
< Referrer-Policy: same-origin
< Cross-Origin-Opener-Policy: same-origin
< 
{ [9 bytes data]
100     9  100     9    0     0     23      0 --:--:-- --:--:-- --:--:--    45
* Connection #0 to host todoapp-service.todoapp.svc.cluster.local left intact
Health OK

kubectl logs my-cronjob-********** -n mateapp

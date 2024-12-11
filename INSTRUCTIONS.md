In order to launch the DaemonSet and CronJob resources:
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml

In order to check whether the pods launched by the daemonset and the cron job are working correctly check their logs:

kubectl get pods
kubectl logs <pod-name> 

They should look similar to the logs below:

fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/x86_64/APKINDEX.tar.gz
v3.20.3-389-g94afa02110d [https://dl-cdn.alpinelinux.org/alpine/v3.20/main]
v3.20.3-392-g277800bdf05 [https://dl-cdn.alpinelinux.org/alpine/v3.20/community]
OK: 24171 distinct packages available
(1/8) Installing coreutils-env (9.5-r1)
(2/8) Installing coreutils-fmt (9.5-r1)
(3/8) Installing coreutils-sha512sum (9.5-r1)
(4/8) Installing libacl (2.3.2-r0)
(5/8) Installing libattr (2.5.2-r0)
(6/8) Installing skalibs (2.14.1.1-r0)
(7/8) Installing utmps-libs (0.1.2.2-r1)
(8/8) Installing coreutils (9.5-r1)
Executing busybox-1.36.1-r29.trigger
OK: 15 MiB in 32 packages
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Host todoapp-service.mateapp.svc.cluster.local:80 was resolved.
* IPv6: (none)
* IPv4: 10.96.213.32
*   Trying 10.96.213.32:80...
* Connected to todoapp-service.mateapp.svc.cluster.local (10.96.213.32) port 80
* using HTTP/1.x
> GET /api/health HTTP/1.1
> Host: todoapp-service.mateapp.svc.cluster.local
> User-Agent: curl/8.11.0
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Date: Wed, 11 Dec 2024 12:52:08 GMT
< Server: WSGIServer/0.2 CPython/3.8.19
< Content-Type: text/plain
< X-Frame-Options: DENY
< Content-Length: 9
< X-Content-Type-Options: nosniff
< Referrer-Policy: same-origin
< Cross-Origin-Opener-Policy: same-origin
< 
{ [9 bytes data]
100     9  100     9    0     0    533      0 --:--:-- --:--:-- --:--:--   562
* Connection #0 to host todoapp-service.mateapp.svc.cluster.local left intact

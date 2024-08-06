# Django ToDo list

This is a todo list web application with basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Explore

Try it out by installing the requirements (the following commands work only with Python 3.8 and higher, due to Django 4):

```
pip install -r requirements.txt
```

Create a database schema:

```
python manage.py migrate
```

And then start the server (default is http://localhost:8000):

```
python manage.py runserver
```

Now you can browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Create a kubernetes manifest for a pod which will containa ToDo app container:

1. Fork this repository.
1. Create a `daemonset.yml` file with a daemonset.
1. DaemonSet requirements:
    1. Container: busyboxplus:curl
    1. Resource requests and limits should be present
    1. Every 5 seconds it should execue a `curl` command to a clusterIp service of a todoapp.
1. Createa a `cronjob.yml` file with a CrobJob manifest.
1. CrobJob requirements:
    1. Container: `busyboxplus:curl`
    1. Resource requests and limits
    1. Every 4 minutes it should call a `/api/health` endpoint of todoapp via a clusterIp service.
    1. Should keep 10 successful runs in history
    1. Should keep 5 failed runs in history
    1. Should have a `concurrencyPolicy` set to `Allow`
1. Both new manifests should belong to `mateapp` namespace
1. `README.md` should be updated with the instructions on how to deploy `daemonset.yml` and `cronjob.yml` to the cluster.
1. `README.md` should be updated with the instructions on how to validate the solution. (Logs for the `daemonset` and `cronjob` should be present)
1. Create PR with your changes and attach it for validation on a platform.

---

## Deploying
Make sure you have the `mateapp` namespace created:

```bash
kubectl create namespace mateapp
```
Use:
```bash
kubectl apply -f .infrastructure/namespace.yml -f .infrastructure/deployment.yml -f .infrastructure/hpa.yml -f .infrastructure/clusterIp.yml
```
### Create daemonset
```bash
kubectl apply -f .infrastructure/daemonset.yml
```
Validation:
```bash
kubectl get pods -n mateapp
```

```
NAME                      READY   STATUS    RESTARTS   AGE
todoapp-deamonset-w6brm   1/1     Running   0          12s
```
```bash
kubectl logs todoapp-deamonset-w6brm -n mateapp
```

```
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/x86_64/APKINDEX.tar.gz
(1/10) Installing ca-certificates (20240226-r0)
(2/10) Installing brotli-libs (1.1.0-r2)
(3/10) Installing c-ares (1.28.1-r0)
(4/10) Installing libunistring (1.2-r0)
(5/10) Installing libidn2 (2.3.7-r0)
(6/10) Installing nghttp2-libs (1.62.1-r0)
(7/10) Installing libpsl (0.21.5-r1)
(8/10) Installing zstd-libs (1.5.6-r0)
(9/10) Installing libcurl (8.8.0-r0)
(10/10) Installing curl (8.8.0-r0)
Executing busybox-1.36.1-r29.trigger
Executing ca-certificates-20240226-r0.trigger
OK: 13 MiB in 24 packages
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3747  100  3747    0     0   519k      0 --:--:-- --:--:-- --:--:--  609k
<!DOCTYPE html>
<html lang="en">

<head>
  <!-- Basic Page Needs
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <meta charset="utf-8">
  <title>Djodolist</title>
  <meta name="description" content="Small todolist app.">
  <meta name="author" content="Christian Rotzoll">
  <!-- Mobile Specific Metas
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  <!-- FONT
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <link href='http://fonts.googleapis.com/css?family=Raleway:400,300,600' rel='stylesheet' type='text/css'>

  <!-- CSS
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <link rel="stylesheet" type='text/css' href="https://cdnjs.cloudflare.com/ajax/libs/normalize/3.0.2/normalize.min.css">
  <link rel="stylesheet" type='text/css' href="https://cdnjs.cloudflare.com/ajax/libs/skeleton/2.0.4/skeleton.min.css">
  <link rel="stylesheet" type='text/css' href="/static/css/custom.css">

  <!-- Scripts
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
  <script type="text/javascript" src="/static/js/site.js"></script>

  <!-- Favicon
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <link rel="icon" type="image/png" href="/static/images/favicon.png" />
</head>

<body>
  <!-- Primary Page Layout
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
  <div class="container">
    <!-- Navigation
    –––––––––––––––––––––––––––––––––––––––––––––––––– -->
    <div class="navbar-spacer"></div>
    <nav class="navbar">
      <div class="container">
        <ul class="navbar-list">
          <li class="navbar-item"><a class="navbar-link" href="/">Djodolist</a></li>

          <li class="navbar-item">
            <a class="navbar-link" href="/auth/login/">Login</a>
        </ul>
      </div>
    </nav>

<section class="header">
  <h2 class="title">Dead simple Todolists.</h2>
  <div class="row">
    <div class="three columns value-prop"></div>
    <div class="six columns">
      <form action="/todolist/new/" method=post>
        <input type="hidden" name="csrfmiddlewaretoken" value="DCYaBNtRuogejSqfe01OAnlpEmHh9625tizuqHz4DOuIY1djQLNuMeFBaV42qwel">
        <dl>
          <dd><tr>
    <th></th>
    <td>

      <input type="text" name="description" class="u-full-width" placeholder="Enter your todo" maxlength="128" required id="id_description">




    </td>
  </tr>
          <dt><input type="submit" class="button button-primary" value="Start one now">
        </dl>
      </form>
    </div>
  </div>
</section>

  </div>
  <!-- End Document
  –––––––––––––––––––––––––––––––––––––––––––––––––– -->
</body>

</html>
```

### Create cronjob

```bash
kubectl apply -f .infrastructure/cronjob.yml
```

Validation:

```bash
kubectl describe cronjob healthcheck -n mateapp
```

```
Events:
  Type    Reason            Age    From                Message
  ----    ------            ----   ----                -------
  Normal  SuccessfulCreate  9m46s  cronjob-controller  Created job healthcheck-28658272
  Normal  SawCompletedJob   9m38s  cronjob-controller  Saw completed job: healthcheck-28658272, status: Complete
  Normal  SuccessfulCreate  5m46s  cronjob-controller  Created job healthcheck-28658276
  Normal  SawCompletedJob   5m38s  cronjob-controller  Saw completed job: healthcheck-28658276, status: Complete
  Normal  SuccessfulCreate  106s   cronjob-controller  Created job healthcheck-28658280
  Normal  SawCompletedJob   94s    cronjob-controller  Saw completed job: healthcheck-28658280, status: Complete
```

```bash
kubectl get pods -n mateapp
```

```
NAME                         READY   STATUS      RESTARTS   AGE
healthcheck-28658272-ggsd6   0/1     Completed   0          11m
healthcheck-28658276-cg79h   0/1     Completed   0          7m1s
healthcheck-28658280-4fjqh   0/1     Completed   0          3m1s
todoapp-deamonset-w6brm      1/1     Running     0          18m
```

```bash
kubectl logs healthcheck-28658272-ggsd6 -n mateapp
```

```
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/x86_64/APKINDEX.tar.gz
(1/10) Installing ca-certificates (20240226-r0)
(2/10) Installing brotli-libs (1.1.0-r2)
(3/10) Installing c-ares (1.28.1-r0)
(4/10) Installing libunistring (1.2-r0)
(5/10) Installing libidn2 (2.3.7-r0)
(6/10) Installing nghttp2-libs (1.62.1-r0)
(7/10) Installing libpsl (0.21.5-r1)
(8/10) Installing zstd-libs (1.5.6-r0)
(9/10) Installing libcurl (8.8.0-r0)
(10/10) Installing curl (8.8.0-r0)
Executing busybox-1.36.1-r29.trigger
Executing ca-certificates-20240226-r0.trigger
OK: 13 MiB in 24 packages
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0   2766      0 --:--:-- --:--:-- --:--:--  3000
Health OK
```

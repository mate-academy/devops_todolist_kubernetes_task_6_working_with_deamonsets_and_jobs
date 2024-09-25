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

## Deploy daemonset and cronjob
To deploy daemonset and cronjob use command: 

    kubectl apply -f .infrastructure/daemonset.yml 

and 

    kubectl apply -f .infrastructure/cronjob.yml 

## Validate the solution
Logs from daemonset: 

    kubectl logs todoapp-hr2dn

        100  3747  100  3747    0     0  19338      0 --:--:-- --:--:-- --:--:-- 39861
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    <!DOCTYPE html>
    <html lang="en">

    <head>
    ...
    </head>

    <body>
    ...
    </body>

    </html>
    100  3747  100  3747    0     0  19187      0 --:--:-- --:--:-- --:--:-- 39031

and logs from cronjob 

    kubectl logs cron-log-28787924-5sncb

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
    100     9  100     9    0     0     82      0 --:--:-- --:--:-- --:--:--   818
    Health OK

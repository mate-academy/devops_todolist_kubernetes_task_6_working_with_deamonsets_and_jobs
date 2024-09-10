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


# How to deploy DaemonSet and CronJob

**Deploy the DaemonSet:** The DaemonSet will run a busyboxplus:curl container on every node in the cluster. The container runs a while loop with a curl command to test the health of the ToDo application.

To apply the DaemonSet to your Kubernetes cluster:

 ``` 
kubectl apply -f .infrastructure/daemonset.yml -n mateapp
 ```

To check if the DaemonSet is running, use the following command:

```
kubectl get daemonset -n mateapp
```

You can also check the logs for the DaemonSet by using the following command:

```
kubectl logs -l app=busybox -n mateapp
```

Deploy the CronJob: The CronJob will run every 4 minutes and call the /api/health endpoint of the ToDo application using a curl command.

To apply the CronJob to your Kubernetes cluster:

```
kubectl apply -f .infrastructure/cronjob.yml -n mateapp
```

To check if the CronJob is running, use the following commands:

```
kubectl get cronjobs -n mateapp
kubectl get jobs -n mateapp
```

You can also check the logs for the most re
```
kubectl logs job/todoapp-cronjob-<job-id> -n mateapp
```
Replace <job-id> with the ID of the running CronJob.

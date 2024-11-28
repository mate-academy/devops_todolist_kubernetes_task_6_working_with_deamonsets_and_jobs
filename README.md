# Django ToDo list

This is a to-do list web application with the basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

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

You can now browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Create a Kubernetes manifest for a pod that will contain a ToDo app container:

1. Fork this repository.
1. Create a `daemonset.yml` file with a daemonset.
1. DaemonSet requirements:
    1. Container: `busyboxplus:curl`
    1. Resource requests and limits should be present
    1. Every 5 seconds it should execute a `curl` command to a clusterIp service of a todoapp.
1. Create a `cronjob.yml` file with a CrobJob manifest.
1. CrobJob requirements:
    1. Container: `busyboxplus:curl`
    1. Resource requests and limits
    1. Every 4 minutes it should call a `/api/health` endpoint of todoapp via a clusterIp service.
    1. Should keep 10 successful runs in history
    1. Should keep 5 failed runs in history
    1. Should have a `concurrencyPolicy` set to `Allow`
1. Both new manifests should belong to `mateapp` namespace
1. Create the `INSTRUCTION.md` file
1. `INSTRUCTION.md` should contain the instructions on how to deploy `daemonset.yml` and `cronjob.yml` to the cluster.
1. `INSTRUCTION.md` should be updated with the instructions on how to validate the solution. (Logs for the `daemonset` and `cronjob` should be present)
1. Create PR with your changes and attach it for validation on a platform.

## Apply DaemonSet and CronJob

To deploy the DaemonSet and CronJob to the cluster, you can use the following commands:

```bash
kubectl apply -f .infrastructure/daemonset.yml -n mateapp
kubectl apply -f .infrastructure/cronjob.yml -n mateapp
```

## Validate the Solution

To validate the solution, you can check the logs for the DaemonSet and CronJob:

```bash
kubectl logs -l app=curl-daemonset
kubectl logs -l job-name
```

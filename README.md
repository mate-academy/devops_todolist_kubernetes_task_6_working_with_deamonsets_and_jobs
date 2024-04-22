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

## To deploy the DaemonSet to your Kubernetes cluster, follow these steps:
$kubectl apply -f daemonset.yml
Check if the DaemonSet has been successfully deployed and all pods are running:
$kubectl get daemonset -n mateapp

## To deploy the CronJob, use the following steps:
$kubectl apply -f cronjob.yml
$kubectl get cronjob -n mateapp

## Validating:
First, find the name of one of the pods created by the DaemonSet:
$kubectl get pods -n mateapp -l app=todoapp-monitor
Replace <pod-name> with the name of the pod from the previous command:
$kubectl logs <pod-name> -n mateapp
This command will display the output of the curl commands being executed within the pod.
To see the instances of the CronJob that have run:
$kubectl get jobs -n mateapp -l job-name=todoapp-health-check
Pick a job from the list and replace <job-name> with its name to view the logs:
$kubectl logs job/<job-name> -n mateapp
This will display the results of the health check endpoint being accessed by the CronJob.

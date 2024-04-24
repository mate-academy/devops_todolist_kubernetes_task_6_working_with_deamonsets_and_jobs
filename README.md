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


In order to deploy `daemonset.yml` and `cronjob.yml` I changed the next files:
clusterIp.yml, deployment.yml, hpa.yml, namespace.yml
Go to the directory .infrastructure
At first, create namespace 'mateapp' and execute the command
kubectl apply -f namespace.yml

Then you can configure your context and set current namespace
kubectl config set-context --current --namespace=mateapp

Apply the next yml-files
kubectl apply -f deployment.yml
kubectl apply -f hpa.yml
kubectl apply -f clusterIp.yml

In order to check pods with the application you should execute the command
kubectl get pods

In order to check current services for current namespace you should execute the command
kubectl get service

Now, we have pods with the application and clusterIp service

In order to start daemonset you should apply the next yaml-file
kubectl apply -f daemonset.yml

In order to check current daemonset
kubectl get daemonset

Then, execute the command
kubectl get pods
you can see the name of daemonset pod

In order to check logs of daemonset
kubectl logs <name of daemonset pod>


In order to start cronjob you should apply the next yaml-file
kubectl apply -f cronjob.yml

In order to review
kubectl get cronjobs

In order to check logs of cronjob
kubectl logs <name of cronjob pod>


# Django ToDo list

This is a to-do list web application with the basic features of most web apps, i.e., accounts/login, API, and interactive UI. To do this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Explore

Try it out by installing the requirements (the following commands work only with Python 3.8 and higher, due to Django 4):

```sh
pip install -r requirements.txt
```

Create a database schema:

```sh
python manage.py migrate
```

And then start the server (default is <http://localhost:8000>):

```sh
python manage.py runserver
```

You can now browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Deployment

To deploy app you need to enter following command:

```sh
kubectl apply -f .infrastructure/daemonset.yml && \
kubectl apply -f .infrastructure/cronjob.yml
```

## Validation

To validate you need:

1. Get all **jobs** in **namespace**:

    ```sh
    kubectl get jobs -n mateapp
    ```

2. Check logs for the last **job**:

    ```sh
    kubectl logs job/todoapp-cronjob-<your_job_hash> -n mateapp
    ```

3. Get all **pods** to find your daemonset pod:

    ```sh
    kubectl get pods -n mateapp
    ```

4. Check logs for your **daemonset** pod:

    ```sh
    kubectl logs <your_daemonset_pod_name> -n mateapp
    ```
---

#### Solution

---

I. Deploying the controllers

*terminal is opened in project root folder


1. **created** **mateapp**  **namespace**

    ```
    kubectl apply -f .\.infrastructure\namespace-mateapp.yml
    ```
2. **created** **todoapp**  **namespace**

    ```
    kubectl apply -f .\.infrastructure\namespace.yml
    ```
3. **created the** **deployment**  **service**

    ```
    kubectl apply -f ..infrastructure\deployment.yml
    ```
4. **created** **horizontal autoscaler**

    ```
    kubectl apply -f ..infrastructure\hpa.yml
    ```
5. **created the** **clusterIP**  **service**

    ```
    kubectl apply -f ..infrastructure\clusterIP.yml
    ```
6. **created** **daemonset**  **controller**

    I created script which would call clusterIp service from curlimage shell in a loop every 5 sec

    ```
    kubectl apply -f .\.infrastructure\daemonset.yml
    ```

    validation:

    1. `kubectl get pods -n mateapp`

        ```
        NAME                         READY   STATUS      RESTARTS   AGE
        daemonsetcurl-x482c          1/1     Running     0          24m
        ```
    2. `kubectl logs daemonsetcurl-x482c -n mateapp`

        ```
          % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                         Dload  Upload   Total   Spent    Left  Speed
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
        100  3747  100  3747    0     0  1231k      0 --:--:-- --:--:-- --:--:-- 1829k
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
                <input type="hidden" name="csrfmiddlewaretoken" value="2sRK3EDrbb44ir0O1p0wrgWGZ42TsnugTCbYuAECGE7iGzRtj3Tf7cxHYtW1tckY">
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
7. **created** **cronjob**  **controller**

    ```
    kubectl apply -f .\.infrastructure\cronjob.yml
    ```

    * to see the details of our cronjob and its working sequence with creating/deleting pods actions and the job status

      `kubectl describe cronjob healthcheck -n mateapp`

      * below events are shown with 1 m interval for test
      * here we see tht succesfull jobs older then 10th are deleted as designed to

      ```
      Events:
        Type    Reason            Age                 From                Message
        ----    ------            ----                ----                -------
        Normal  SuccessfulCreate  56m                 cronjob-controller  Created job healthcheck-28639832
        Normal  SawCompletedJob   56m                 cronjob-controller  Saw completed job: healthcheck-28639832, status: Complete
        Normal  SuccessfulCreate  52m                 cronjob-controller  Created job healthcheck-28639836
        Normal  SawCompletedJob   52m                 cronjob-controller  Saw completed job: healthcheck-28639836, status: Complete
        Normal  SuccessfulCreate  48m                 cronjob-controller  Created job healthcheck-28639840
        Normal  SawCompletedJob   48m                 cronjob-controller  Saw completed job: healthcheck-28639840, status: Complete
        Normal  SuccessfulCreate  44m                 cronjob-controller  Created job healthcheck-28639844
        Normal  SawCompletedJob   44m                 cronjob-controller  Saw completed job: healthcheck-28639844, status: Complete
        Normal  SuccessfulCreate  43m                 cronjob-controller  Created job healthcheck-28639845
        Normal  SawCompletedJob   43m                 cronjob-controller  Saw completed job: healthcheck-28639845, status: Complete
        Normal  SuccessfulCreate  42m                 cronjob-controller  Created job healthcheck-28639846
        Normal  SawCompletedJob   42m                 cronjob-controller  Saw completed job: healthcheck-28639846, status: Complete
        Normal  SuccessfulCreate  41m                 cronjob-controller  Created job healthcheck-28639847
        Normal  SawCompletedJob   41m                 cronjob-controller  Saw completed job: healthcheck-28639847, status: Complete
        Normal  SuccessfulCreate  40m                 cronjob-controller  Created job healthcheck-28639848
        Normal  SawCompletedJob   40m                 cronjob-controller  Saw completed job: healthcheck-28639848, status: Complete
        Normal  SuccessfulCreate  39m                 cronjob-controller  Created job healthcheck-28639849
        Normal  SawCompletedJob   39m                 cronjob-controller  Saw completed job: healthcheck-28639849, status: Complete
        Normal  SuccessfulDelete  37m                 cronjob-controller  Deleted job healthcheck-28639832
        Normal  SuccessfulDelete  36m                 cronjob-controller  Deleted job healthcheck-28639836
        Normal  SawCompletedJob   35m (x4 over 38m)   cronjob-controller  (combined from similar events): Saw completed job: healthcheck-28639853, status: Complete
        Normal  SuccessfulDelete  35m                 cronjob-controller  Deleted job healthcheck-28639840
        Normal  SuccessfulCreate  95s (x38 over 38m)  cronjob-controller  (combined from similar events): Created job healthcheck-28639887
      ```
    * to see server response of each job

      1. `kubectl get pods -n mateapp`

          ```
          NAME                         READY   STATUS      RESTARTS   AGE
          daemonsetcurl-x482c          1/1     Running     0          24m
          healthcheck-28639884-kq2vx   0/1     Completed   0          9m27s
          healthcheck-28639885-pp65m   0/1     Completed   0          8m27s
          healthcheck-28639886-5wbkw   0/1     Completed   0          7m27s
          healthcheck-28639887-k2snh   0/1     Completed   0          6m27s
          healthcheck-28639888-t5zkn   0/1     Completed   0          5m27s
          healthcheck-28639889-fsxjw   0/1     Completed   0          4m27s
          healthcheck-28639890-fcphr   0/1     Completed   0          3m27s
          healthcheck-28639891-hs2m4   0/1     Completed   0          2m27s
          healthcheck-28639892-jwgsp   0/1     Completed   0          87s
          healthcheck-28639893-89wt5   0/1     Completed   0          27s
          ```
      2. `kubectl logs healthcheck-28639891-hs2m4`

          ```
            % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                           Dload  Upload   Total   Spent    Left  Speed
          100     9  100     9    0     0   4833      0 --:--:-- --:--:-- --:--:--  9000
          Health OK
          ```
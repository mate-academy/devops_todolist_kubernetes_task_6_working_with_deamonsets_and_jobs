To deploy daemonset.yml run the following command:
    kubectl apply -f .infrastructure/deamonset.yml
To deploy cronjob.yml run:
    kubectl apply -f .infrastructure/cronjob.yml
To validate the solution check the pods running  within the mateapp namespace:
    kubectl get pods -n mateapp
You should get something like this:
NAME                             READY   STATUS    RESTARTS   AGE
todoapp-cronjob-29005588-rh7j8   1/1     Running   0          3m43s
todoapp-daemon-ns5kr             1/1     Running   0          18m

To check logs for cronjob use the cronjob pod name:
    kubectl logs todoapp-cronjob-29005588-rh7j8 -n mateapp
You'll get something like the following:
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0    537      0 --:--:-- --:--:-- --:--:--   562
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0     91      0 --:--:-- --:--:-- --:--:--    92
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0   2796      0 --:--:-- --:--:-- --:--:--  4500
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed


To check the daemonset task use the corresponding name:
    kubectl logs todoapp-daemon-ns5kr -n mateapp

100  3747  100  3747    0     0   554k      0 --:--:-- --:--:-- --:--:--  609k
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



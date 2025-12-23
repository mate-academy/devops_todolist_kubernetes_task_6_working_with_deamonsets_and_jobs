To deploy daemonset and cronjob run below from project folder /.infrastructure folder:

kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml


to get cronjob logs:

kubectl get pods -n mateapp
and check logs for some completed todoapp-cronjob-XXXX

e.g.

kubectl logs todoapp-cronjob-29441940-sjbch -n mateapp
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0   2257      0 --:--:-- --:--:-- --:--:--  3000
Health OK%                                                                              


kubectl logs todoapp-cronjob-29441968-5nnsp -n mateapp
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100     9  100     9    0     0   1568      0 --:--:-- --:--:-- --:--:--  1800
Health OK%          


to get daemonset logs:

kubectl get pods -n mateapp

and check logs for some running todoapp-XXXX (not cron job)

e.g. 

kubectl logs todoapp-kv85r -n mateapp
</body>

</html>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3747  100  3747    0     0   579k      0 --:--:-- --:--:-- --:--:--  609k
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
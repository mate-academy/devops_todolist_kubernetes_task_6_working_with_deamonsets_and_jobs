# Deployment & Validation Instructions

## Prerequisites

- `kubectl` installed and configured
- Access to the cluster
- `mateapp` namespace exists or will be created

---

## 1. Create the Namespace (if it doesn't exist)

```bash
kubectl create namespace mateapp
```

---

## 2. Deploy the DaemonSet

```bash
kubectl apply -f daemonset.yml
```

Verify the DaemonSet was created and pods are running:

```bash
kubectl get daemonset -n mateapp
kubectl get pods -n mateapp -l app=hw-mate-server
```

Expected output: one pod per node in the cluster, all with `Running` status.

---

## 3. Deploy the CronJob

```bash
kubectl apply -f cronjob.yml
```

Verify the CronJob was created:

```bash
kubectl get cronjob -n mateapp
```

---

## 4. Validate the DaemonSet

### Check pod logs

Get the name of a DaemonSet pod:

```bash
kubectl get pods -n mateapp -l app=hw-mate-server
```

Then view the logs:

```bash
kubectl logs <pod-name> -n mateapp
```

**Expected output** — repeated every 5 seconds, showing the HTML response from the todoapp:
```
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
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
        <input type="hidden" name="csrfmiddlewaretoken" value="Bn45BCLETBkbexitocgRhsFGXqO9CO2fhMOpbdDictJBrA9SJIO1e99azrJl0Gyh">
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
100   3747 100   3747   0      0  39815      0                              0
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
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
        <input type="hidden" name="csrfmiddlewaretoken" value="nZbk5v9NsQMUR8tslJxOQuhmgGFRUTWZa4Dy1bJW4x7up85o3TEeg0F8MJsNLi9R">
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
100   3747 100   3747   0      0   9008      0                              0
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100   3747 100   3747   0      0  73056      0                              0
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
        <input type="hidden" name="csrfmiddlewaretoken" value="VBlmRGG0Q7FwyLUU4peU7Qb5Kv0AOFcKc5jlcDmhqVRYgy7AduR5q8uhFLyfNOAy">
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
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
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
        <input type="hidden" name="csrfmiddlewaretoken" value="sfReICZTb7KKl57hzQKjjmxe26F13myJC4W221KzsMjd3q07O05s3Ll2921kwpuJ">
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
100   3747 100   3747   0      0  23200      0                              0
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100   3747 100   3747   0      0  15935      0                              0
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
        <input type="hidden" name="csrfmiddlewaretoken" value="5gFzpv472he871K1GvnmA5O1bgLp4ReZ2VoCkkAKZSvRWRefXYlVDb2fmSn8hR1M">
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

<!DOCTYPE html>
<html lang="en">
...
100   3747 100   3747   0      0  ...
```
---

## 5. Validate the CronJob

The CronJob runs every 4 minutes. Wait for the first job to trigger, or manually create a job to test immediately:

```bash
kubectl create job --from=cronjob/health-cronjob health-cronjob-manual-test -n mateapp
```

List completed jobs:

```bash
kubectl get jobs -n mateapp
```

Get the pod name for a completed job run:

```bash
kubectl get pods -n mateapp -l job-name=<job-name>
```

View the logs:

```bash
kubectl logs <pod-name> -n mateapp
```

**Expected output:**

```
  % Total    % Received % Xferd  Average Speed  Time    Time    Time   Current
                                 Dload  Upload  Total   Spent   Left   Speed
100      9 100      9   0      0     22      0                              0
Health OK%
```

### Check job history

The CronJob retains up to **10 successful** and **5 failed** job runs. To inspect history:

```bash
kubectl get jobs -n mateapp
```
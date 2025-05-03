Create the `INSTRUCTION.md` file
1. `INSTRUCTION.md` should contain the instructions on how to deploy `daemonset.yml` and `cronjob.yml` to the cluster.
1. `INSTRUCTION.md` should be updated with the instructions on how to validate the solution. (Logs for the `daemonset` and `cronjob` should be present)

## Prerequisites   
Use command:  
`alias kub=kubectl`  
this will make less typing for local session  

Apply manifests in following order:  
 - `kub apply -f ./.infrastructure/namespace.yml` 
 - `kub config set-context --current --namespace=mateapp`  
 - `kub apply -f ./.infrastructure/clusterIp.yml`  
 - `kub apply -f ./.infrastructure/nodeport.yml`   
 - `kub apply -f ./.infrastructure/busybox.yml`  
 - `kub apply -f ./.infrastructure/deployment.yml`  
 - `kub apply -f ./.infrastructure/hpa.yml`  

## 1. Deploy Daemon and CronJob:
Apply appropriate manifests:  

 - `kub apply -f ./.infrastructure/daemonset.yml`
 - `kub apply -f ./.infrastructure/cronjob.yml`

 ## 2. Validation

    ### CronJob  
    - Виконай наступну команду, щоб отримати список Jobs:  
        `kub get pods`  
        > NAME                       STATUS    COMPLETIONS   DURATION   AGE
        > busybox                               1/1     Running        2 (10m ago)   43m
        >todoapp-cronjob-29104924-qqj8k        0/1     Completed      0             3m5s
        >todoapp-daemon-l6z8x                  1/1     Running        0             25m
        >todoapp-deployment-74bddf9fc7-knzpc   1/1     Running        0             43m
        >todoapp-deployment-74bddf9fc7-rr4qg   1/1     Running        0             42m
    - Знайшовши необхідне ім'я, виконай команду:  
        `kub logs todoapp-cronjob-29104924-qqj8k`  
        і отримай відповідь:  
        `Health OK`
    - Перевір так само Daemon, знайшовши його назву в списку:  
        `kub logs todoapp-daemon-l6z8x`  
        і отримай відповідь:  
        ```
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
                    <input type="hidden" name="csrfmiddlewaretoken" value="HcUchDhYodKe1pGZfl7JvpBuDuGE3w6f3RAsVS5ri8zHw9G3kkkRYWNdW1J7mRlJ">
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
                    <input type="hidden" name="csrfmiddlewaretoken" value="snX6ffQCF1WVwQ34jlLdWpXtsOUJLmHJGpALqwkJHJrPElY7MBrzwbLVyEGRvHMY">
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
                    <input type="hidden" name="csrfmiddlewaretoken" value="xfEuIZVRjvneNvptSwOofKyXkURNGWey4t04lOKTfHZ3FtXiMH5iNOaYheYedy8I">
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
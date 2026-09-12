Use the following instructions below:

Prerequisites

* Kubernetes cluster
* Existing todoapp deployment
* Existing ClusterIP service for todoapp
* Namespace `mateapp`

1. All instructions:
    - how to deploy daemonset.yml and cronjob.yml to the cluster:
        kubectl apply -f .infrastructure/daemonset.yml
        kubectl apply -f .infrastructure/cronjob.yml
        

    - how to validate the solution (Logs for the daemonset and cronjob should be present)
        -- receive info for daemonset
        kubectl get daemonsets -n mateapp
        kubectl get pods -n mateapp -l app=busyboxplus
        kubectl logs -n mateapp -l app=busyboxplus --tail=100

        -- receive info for cronjob
        kubectl get cronjob -n mateapp
        kubectl get jobs -n mateapp
        kubectl logs -n mateapp -l job-name=<job-name>
        
        **Expected Log Output for daemonset:**

        --- Sat Sep 12 19:26:39 UTC 2026 ---
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                        Dload  Upload   Total   Spent    Left  Speed
        HTTP/1.1 200 OK
        Date: Sat, 12 Sep 2026 19:26:40 GMT
        Server: WSGIServer/0.2 CPython/3.8.19
        Content-Type: text/html; charset=utf-8
        X-Frame-Options: DENY
        Vary: Cookie
        Content-Length: 3747
        X-Content-Type-Options: nosniff
        Referrer-Policy: same-origin
        Cross-Origin-Opener-Policy: same-origin
        Set-Cookie:  csrftoken=NwQBoQgFgxbIw9uXnOyPE36kAqwbRJFY; expires=Sat, 11 Sep 2027 19:26:40 GMT; Max-Age=31449600; Path=/; SameSite=Lax

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
                <input type="hidden" name="csrfmiddlewaretoken" value="N2GTOIp8R2L9CbvAOszrn9ueV1fAZE5Yqomk2ovDXpMHYaPn16X6R2qolhBBGdAM">
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
        100  3747  100  3747    0     0  35266      0 --:--:-- --:--:-- --:--:--  159k
        --- Sat Sep 12 19:26:45 UTC 2026 ---



        **Expected Log Output for cronjob:**

        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                        Dload  Upload   Total   Spent    Left  Speed
        HTTP/1.1 200 OK
        100     9  100     9    0     0     88      0 --:--:-- --:--:-- --:--:--   173
        Date: Sat, 12 Sep 2026 19:36:01 GMT
        Server: WSGIServer/0.2 CPython/3.8.19
        Content-Type: text/plain
        X-Frame-Options: DENY
        Content-Length: 9
        X-Content-Type-Options: nosniff
        Referrer-Policy: same-origin
        Cross-Origin-Opener-Policy: same-origin

        Health OK
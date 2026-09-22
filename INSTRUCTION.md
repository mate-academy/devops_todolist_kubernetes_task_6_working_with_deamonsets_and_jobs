# Deployment Instructions

## Deploy

```bash
*kubectl* apply -f daemonset.yml

*kubectl* apply -f cronjob.yml
```

## Check resources

```bash
*kubectl* get daemonset -n mateapp

*kubectl* get cronjob -n mateapp

*kubectl* get jobs -n mateapp

*kubectl* get pods -n mateapp -o wide
```

## DaemonSet logs

```bash
*kubectl* logs -n mateapp -l app=todoapp
```

Follow logs:

```bash
*kubectl* logs -n mateapp -l app=todoapp -f
```

## CronJob logs

```bash
*kubectl* get jobs -n mateapp

*kubectl* get pods -n mateapp
```

## Check resources

```bash
*kubectl* describe daemonset todoapp-daemonset -n mateapp

*kubectl* describe cronjob todoapp-cronjob -n mateapp
```

## Cleanup

```bash
*kubectl* delete -f daemonset.yml

*kubectl* delete -f cronjob.yml
```


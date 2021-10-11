# Task: Create Cronjobs in Kubernetes

There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:



Create a cronjob named nautilus.

Set schedule to */5 * * * *.

Container name should be cron-nautilus.

Use httpd image with latest tag only and remember to mention the tag i.e httpd:latest.

Run a dummy command echo Welcome to xfusioncorp!.

Ensure restart policy is OnFailure.

# Solution

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: nautilus
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-nautilus
            image: httpd:latest
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - echo Welcome to xfusioncorp!
          restartPolicy: OnFailure
```
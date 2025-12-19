# CronJobs

CronJobs are scheduled repeting tasks.
Given this job,

```job-definition.yaml
apiVersion: batch/v1
kind: Job
metadata:
    name: math-add-job
spec:
    template:
        spec:
            containers:
            - name: math-add
              image: ubuntu
              command: ['expr', '3', '+', '2']
            restartPolicy: Never
```

we define a cronJob like:

```cron-job-definition.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
    name: reporting-cron-job
spec:
    schedule: "*/1 * * * *"
    jobTemplate:
        spec:
        template:
            spec:
                containers:
                - name: math-add
                  image: ubuntu
                  command: ['expr', '3', '+', '2']
                restartPolicy: Never
```

The  `spec.schedule` property defines a cron expression to schedule the jobTemplate execution.

When the cronJob is defined, create it

```shell
kubectl create -f cron-job-definition.yaml
```

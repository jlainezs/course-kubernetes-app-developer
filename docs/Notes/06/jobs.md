# Jobs

Batch processes that starts and finish.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: math-pod
spec:
    containers:
    - name: math-add
      image: ubuntu
      command: ['expr', '3', '+', '2']
```

This container:
- is started
- do some operation
- finish

but Kubernetes keeps restarting it until a threshold is reached.

That's because the pod default restart policy is set to  `Always` . To change this behavioud, just set the restart policy to  `Never` or   `Failure`:

```yaml
...
containers:
- name: math-add
  ...
restartPolicy: Never
```

## Work in parallel

Starting from a pod definition file, we create a job definition yaml.

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

To operate this job:
```shell
kubectl create -f ./job-definition.yaml
# inspect jobs
kubectl get jobs
# inspect the pod status
kubectl get pods
# inspect result, assuming it is in the pod stdout
kubectl logs <pod_name>
# clean resources
kubectl delete job math-add-job
```

Usually, output is persisted in a volume, database or other persistent media.

To start more pods to:

```job-definition.yaml
...
spec:
    completions: 3
    template:
    ...
```

With this setup pods are create one after the other until the number of `completed` pods reaches the  `completions` property. So, if a pod fails, with this setup, we end with at least 4 pods created.

To create pods in parallel we set the property  `parallelism` to the number of pods to create at once:

```job-definition.yaml
...
spec:
    completions: 3
    parallelism: 3
    template:
    ...
```

This setup will start creating three pods. If one pod fails, kubernetes launches an additional pod. This process is repeated until the `completed` pods reaches the  `completions` property.
 
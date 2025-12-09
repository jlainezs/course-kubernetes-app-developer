# Multi-Container Pods

Keep containers tight togheter in the same pod. This allows to implement microservices and scale up and down applications and services.

## Definition

Use the spec.containers array to describe as many containers as needed. The following example defines two containers to be run on a pod.

```multicontainer-pod.yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: wimple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
        - containerPort: 8080
    - name: main-app
      image: main-app
```
## Design patterns

- Co-located containers: a container provides services for the other container.
- Regular init containers: a container starts services on another container and finishes.
- Sidecar containers

## Co-located containers
There is no particular order on creation and run of containers.

```co-located-containers-pod.yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: wimple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
        - containerPort: 8080
    - name: main-app
      image: main-app
```
## Regular init containers
We define an additional `initContainers` property. When the `wait-for-db-to-start.sh` command ends, then the main container starts.

```co-located-containers-pod.yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: wimple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
        - containerPort: 8080
    initContainers:
    - name: db-checker
      image: busybox
      command: 'wait-for-db-to-start.sh'
```

As initContainers is an array we can define as many containers as needed and they'll be executed in sequence before the `web-app` container.

## Sidecar containers
We define the additional `initContainers` property but we set the `restartPolicy` of them to `Always`

```Sidecar-containers-pod.yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: wimple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
        - containerPort: 8080
    initContainers:
    - name: log-shipper
      image: busybox
      command: 'setup-log-shipper.sh'
      restartPolicy: Always
```

As initContainers is an array we can define as many containers as needed and they'll be executed in sequence before the `web-app` container.

## References

- [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

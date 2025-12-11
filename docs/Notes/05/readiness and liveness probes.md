# Readines and liveness probes

## Pod Status

A Pod's status field is a PodStatus object, which has a phase field.

The phase of a Pod is a simple, high-level summary of where the Pod is in its lifecycle. The phase is not intended to be a comprehensive rollup of observations of container or Pod state, nor is it intended to be a comprehensive state machine.

The number and meanings of Pod phase values are tightly guarded. Other than what is documented here, nothing should be assumed about Pods that have a given phase value.

Here are the possible values for phase:

| Value     | Description|
|-----------|------------|
| Pending   | The Pod has been accepted by the Kubernetes cluster, but one or more of the containers has not been set up and made ready to run. This includes time a Pod spends waiting to be scheduled as well as the time spent downloading container images over the network. |
| Running   | The Pod has been bound to a node, and all of the containers have been created. At least one container is still running, or is in the process of starting or restarting.|
| Succeeded | All containers in the Pod have terminated in success, and will not be restarted.|
| Failed    | All containers in the Pod have terminated, and at least one container has terminated in failure. That is, the container either exited with non-zero status or was terminated by the system, and is not set for automatic restarting.|
| Unknown   | For some reason the state of the Pod could not be obtained. This phase typically occurs due to an error in communicating with the node where the Pod should be running. |

## Conditions

Conditions complement the pod status.

## Readiness Probe

Kubernetes sets the state of a pod to ready as soon as it is created. Application running on the pod may take longer to be ready, and users will be redirected to a pod wich is not yet ready to serve traffic. Readiness Probe is used to check for an application ready.

We declare the readiness prove in the spec.containers section.

```readynessProve.yaml
apiVersion: v1
kind: Pod
metadata:
    name: webapp-1
spec:
    containers:
    - name: webapp-1
      image: webapp-1
      ports:
        - containerPort: 8080
      readinessProbe:
        httpGet:
            path: /api/ready
            port: 8080
```

### Get Probe

```
readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
```
### TCP Probe

```
readinessProbe:
    tcpSocket:
        port: 3306
```
### Exec command probe

```
readinessProbe:
    exec:
        command:
            - cat
            - /app/is_ready
```

## Some control on probes

On probes we can specify:

- ianitialDelaySeconds: seconds to wait before start checking the probe
- periodSeconds: second between probe checks
- failureThreshold: number of failed tests before terminating the pod

```
readinessProbe:
    exec:
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 8
        command:
            - cat
            - /app/is_ready
```
## Liveness Probe

A liveness probe can be configured on the container to periodically test the application health. If the application is not healthy then the container is destroyed and recreated.

```livenessProve.yaml
apiVersion: v1
kind: Pod
metadata:
    name: webapp-1
spec:
    containers:
    - name: webapp-1
      image: webapp-1
      ports:
        - containerPort: 8080
      livenessProbe:
        httpGet:
            path: /api/healthy
            port: 8080
```

### Get Probe

```
livenessProbe:
    httpGet:
        path: /api/ready
        port: 8080
```
### TCP Probe

```
livenessProbe:
    tcpSocket:
        port: 3306
```
### Exec command probe

```
livenessProbe:
    exec:
        command:
            - cat
            - /app/is_ready
```
### Control

```
initialDelaySeconds: 10
periodSeconds: 5
failureThreshold: 8
```

## Reference

- [Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

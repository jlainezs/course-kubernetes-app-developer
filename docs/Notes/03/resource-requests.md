# Resource request

Specify resrouces need to run the pod and define the pod resource limits.

```yaml
...
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
    resources:
        requests:
            memory: "1Gi"
            cpu: 2
        limits:
            memory: "2Gi"
            cpu: 4
```

Requests define the minimum CPU and memory a pod needs, while limits define the pod maximum CPU and memory.

Combinations:
- No requests, no limits
- No requests, limits
- Requests, limits
- Requests, no limits

## No resources
When creating a pod, if aialable resources are less than requested resources then the pod can't be created. Kubernetes will keep the pod in a "Pending state".

### OOM event
When running, a pod is terminated if exceeds the limit memory often.


## Power of 10 vs. power of 2
- G  = 1.000.000.000 bytes, decimal gigabytes, power of 10
- Gi = 1.073.741.824 bytes, binary gigabytes, power of 2

The recommendation to define memory requests is to express units as binary units (Ki, Mi, Gi, Ti, Pi, Ei).

## Default resources

With LimitRange definition we can define default limits and requests for memory and cpu. Those limits are enforced on pod creation, so changes applied to LimitRange are not applied to running pods.

```yaml
apiVersion: v1
kind: LimitRange
metadata:
    name: cpu-resource-constraint
spec:
    limits:
    - default: # refers to limi
        cpu: 500m
      defaultRequest: # refers to request
        cpu: 500m
      max: # refers to limit
        cpu: "1"
      min: # refers to request
        cpu: 100m
      type: Container
```

```yaml
apiVersion: v1
kind: LimitRange
metadata:
    name: memory-resource-constraint
spec:
    limits:
    - default: # refers to limi
        memory: 1Gi
      defaultRequest: # refers to request
        memory: 1Gi
      max: # refers to limit
        memory: 1Gi
      min: # refers to request
        memory: 500Mi
      type: Container
```

## Resource quotas

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
    name: my-resource-quota
spec:
    hard:
        requests.cpu: 4
        requests.memory: 4Gi
        limits.cpu: 10
        limits.memory: 10Gi
```

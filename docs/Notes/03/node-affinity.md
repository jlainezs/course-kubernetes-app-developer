# Node affinity

The main purpose of node affinity is to ensure that pods are hosted on particular nodes.

To select a node using nodeSelector:

```yaml
sepc:
    nodeSelector:
        size: Large
```

To select the same node using affinity:

```yaml
sepc:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelctorTerms:
                - matchExpressions:
                    - key: size
                      operator: In
                      values:
                        - Large
```
To host the pod in a Large or Medium sized node we

```yaml
sepc:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelctorTerms:
                - matchExpressions:
                    - key: size
                      operator: In
                      values:
                        - Large
                        - Mdium
```

To express we wan't use a Small pod


```yaml
sepc:
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelctorTerms:
                - matchExpressions:
                    - key: size
                      operator: NotIn
                      values:
                        - Small
```

## Node affinity types

- requiredDuringSchedulingIgnoredDuringExecution: nodeSelector is checked on pod scheduling to select a node and ignored if the pod is already running (pe. label de node after the pod runs). If no pod satisfies the matchExpression the node is kept on pending.
- preferredDuringSchedulingIgnoredDuringExecution: nodeSelector is  checked on scheduling, but if no node is found, then the pod is scheduled on any node. If the node is running, changes on node labels won't affect running pods.



## References
- [Assigning Pods to Nodes](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
- [Assign Pods to Nodes using Node Affinity](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/)

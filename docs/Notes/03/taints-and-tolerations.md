# Taints and tolerations

Taints and tolerations are used to set restrictions on what pods can be scheduled on a node.

Taints are defined on pods while tolerations are defined on the pod.

It is not warrantied that a pod with certain toleration will be executed on a node with the same taint, as it can be executed on nodes with no taints defined.

Taints and tolerations are not affinity!

Master Node does not accept pods (and it is a good practice not to execute pods in the master node), by defining a taint:

```shell
$ kubectl describe node kubemaster | grep Taint
Taints:       node-role.kubernetes.io/master:NoSchedule
```

## Define taint

```
kubectl taint nodes <node-name> <key> = <value>:<taint-effect>
```

where taint-effect is one of:

- **NoSchedule**: The Kubernetes scheduler will only allow scheduling pods that have tolerations for the tainted nodes.
- **PreferNoSchedule**: The Kubernetes scheduler will try to avoid scheduling pods that don’t have tolerations for the tainted nodes.
- **NoExecute**: Kubernetes will evict the running pods from the nodes if the pods don’t have tolerations for the tainted nodes.

Example:

```
kubectl taint nodes node1 app=blue:NoSchedule
```

## Remove a taint
Add a - at the end of the taint to be removed:
```
kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
```

## Define tolerations

```yaml
apiVersion: v1
kind: Pod
..
spec:
    tolerations:
        - key: "app"
          operator: "Equal"
          value: "blue"
          effect: "NoSchedule"
...
```

# Node selectors
Alters default pod schedule and selects the node where a pod will run.

We need to assign labels to the node

```bash
kubectl label nodes <node_name> <label_key>=<label_value>
```

and then use them in the pod.nodeSelector:

```yaml
sepc:
    nodeSelector:
        size: Large
```

With node selector we can't express complex conditions like 'large or medium'. For more complex node selections, use node affinityand anti-affinity instead of node selector.

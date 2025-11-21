# Edit pods

## Generatre pod yaml definition file
```
kubectl get pod <pod-name> -o yaml > pod-definition.yaml
```

Edit and apply the chnages.

## Modify pod properties

To modify the properties of the pod, you can utilize the ```kubectl edit pod <pod-name>``` command. Please note that only the properties listed below are editable.

    spec.containers[*].image
    spec.initContainers[*].image
    spec.activeDeadlineSeconds
    spec.tolerations
    spec.terminationGracePeriodSeconds

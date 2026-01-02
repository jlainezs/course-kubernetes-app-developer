# Storage in StatefulSets

Storage in StatefulSets may need a different approach. Generally, we don't want that all pods use the same storage (p.e., a database cluster). We need to create volume claims dynamically for each pod, so we add the claim definition in the StatefulSet `spec.volumeClaimTemplates` property.

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
    name: mysql
    labels:
        app: mysql
spec:
    replicas: 3
    selector:
        matchLabels:
            app: mysql
    template:
        metadata:
            labels:
                app: mysql
        spec:
            containers:
            - name: mysql
              image: mysql
              volumeMounts:
              - mountPath: /var/lib/mysql
                name: data-volume
            volumes:
            - name: data-volume
              persistentVolumeClaims:
                claimName: data-volume
    volumeClaimTemplates:
    - metadata:
        name: data-volume
      spec:
        accessModes:
            - ReadWriteOnce
        storageClassName: google-storage
        resources:
            requests:
                storage: 500Mi
```
Note that the `StorageClass` definition file is still needed.

If a pod is recreated (or fails on creation), the new pod is reattached to the previous persistent volume claim.

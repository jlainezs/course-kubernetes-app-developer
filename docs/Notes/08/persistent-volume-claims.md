# Persistent volume claims

- An administrator creates a set of persistent volumes.
- A user creates persistent volumes claims to use the storage.

Kubernetes binds the persistent volumes to claims based on the request and the properties set on the volume.

Additionally, the user can select a specific volume using labels and selectors.

Volumes and claims have a 1 to 1 relation.

## Persistent Volume Claim
Define the claim:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 500Mi
```
create it

```shell
kubectl create -f myclaim.yaml
```
## Delete claims

We can define what happens to the underlying volume when a claim is deleted:

persistentVolumeReclaimPolicy: Retain

By default, a volume is retained until the volume is manually deleted.

persistentVolumeReclaimPolicy: Delete

The volume is deleted.

persistentVolumeReclaimPolicy: Recycle

Data in the volume is cleaned before using the volume on other claims (deprecated).

## Using PVCs in Pods

Once you create a PVC use it in a POD definition file by specifying the PVC Claim name under persistentVolumeClaim section in the volumes section like this:

```
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: myfrontend
          image: nginx
          volumeMounts:
          - mountPath: "/var/www/html"
            name: mypd
      volumes:
        - name: mypd
          persistentVolumeClaim:
            claimName: myclaim
```

The same is true for ReplicaSets or Deployments. Add this to the pod template section of a Deployment on ReplicaSet.


Reference URL: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes
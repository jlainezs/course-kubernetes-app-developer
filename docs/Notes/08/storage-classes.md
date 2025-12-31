# Storage Classes

Storages Classes provides dynamic volume provisioning (see [Persistent Volume in Google Cloud](persistent-volume-in-google-cloud.md) for static volume provision).

## Definition

To enable dynamic volume provision, we need to define storage classes:

```storage-class.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: google-storage
provisioner: kubernetes.io/gce-pd
```

## Usage

With the previous storage class definition, we create a persistent volume claim:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec:
    accessModes:
    - ReadWriteOnce
    storageClassName: google-storage
    resources:
        requests:
            storage: 500Mi
```

The storage class creates a volume automatically when the claim is created, so there's no need to define and manually create the volume.

## Provisioners

There are different storage classes for Azure, AWS, ...

### Parameters

Each provisioner define its own additional parameters.

```storage-class-gc.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: google-storage
provisioner: kubernetes.io/gce-pd
parameters:
    type: pd-standard
    replication-type: none
```

## Reference

- [Storage classes, official documentation](https://kubernetes.io/docs/concepts/storage/storage-classes/)

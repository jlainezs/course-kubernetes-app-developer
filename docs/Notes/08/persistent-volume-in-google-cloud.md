# Persistent volume in Google Cloud

## Static volume provision

First step is to create a Persistent Disk in Google Cloud:

```shell
gcloud compute disks create \
    --size 1GB
    --region eu-west3
    pd-disk
```

Then create the persistent voume:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
    name: pv-vol1
spec:
    accessMode:
        - ReadWriteOnce
    capacity:
        storage: 500Mi
    gcePersistentDisk:
        pdName: pd-disk
        fsType: ext4
```
And now use it as a local persistent volume.

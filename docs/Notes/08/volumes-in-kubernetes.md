# Volumes in Kubernetes

As in Docker containers, Kubernetes pods are transient, as well as its data. If we need to keep data, we attach a volume to the pod, so the pod can use it to store its data.

## Basic volume declaration

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  persistentVolumeReclaimPolicy: Retain
  accessModes:
    - ReadWriteMany
  capacity:
    storage: 100Mi
  hostPath:
    path: /pv/log
```

The following pod uses a dictory volume named `data-volume` defined in the host `/data` directory and accessible though the  `/opt` directory in the pod filesystem.

```
apiVersion: v1
kind: Pod
metadata:
    name: random-number-generator
spec:
    containers:
    - image: alpine
      name: alpine
      command: ["/bin/sh", "-c"]
      args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
      volumeMounts:
      - mountPath: /opt
        name: data-volume
    volumes:
    - name: data-volume
      hostPath:
        path: /data
        type: Directory
```

For Kubernetes clusters with more than one node it is recommended to use an external storage (AWS, Azure, GCE persistent disk, NFS, ...).

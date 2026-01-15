# API Versions

By default, kubernetes is using GA stable versions like `/apps/v1`.

## Use beta or alpha releases
If we want to use an alpha version, we'll note it in the definition yaml:

```yaml
apiVersion: internatl.apiserver.k8s.io/v1alpha1
kind: StorageVersion
metadata:
    name: sv-1
spec: ...
```
## Versioning

In general there are three kind of versions:

- alphaX: early releases. May contain bugs and it could desapear on following releases. Can be enabled via flags.
- betaY: may have minor bugs. It is enabled by default. Will move into GA.
- GA stable.

alpha1 -> alpha2 -> ... -> beta1 -> beta2 -> ... -> GA stable

## Preferred or storage version

In spite we can have enabled several versions, only one can be the preferred or storage version.

When running a command like `kubectl get deployments` we are using the preferred version.

Any created object is converted to the storage version before it is being stored into the etcd database.

To identify the preferred API version, we can examine the API:

```
{
    "kind": "APIGroup",
    "apiVersion": "v1",
    "name": "batch",
    "versions": [
        {
            "groupVersion": "batch/v1",
            "version": "v1"
        },
        {
            "groupVersion": "batch/v1beta1",
            "version": "v1beta1"
        }
    ],
    "preferredVersion": {
        "groupVersion": "batch/v1",
        "version": "v1"
    }
}
```

It is not possible to identify the storage versions using any API query or command. To check storage version we need to query the etcd database.

## Enabling/Disabling API groups

To enable/disable a specific version you must add it to the runtime config parameter of the kube api-server.

```
ExecStart=/usr&local/bin/kube-apiserver \\
    --runtime-config=batch/v2alpha1
```

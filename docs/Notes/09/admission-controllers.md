# Admission Controllers

Every request (pe., to create a pod)

kubelet -> kube-apiserver -> Create pod

In the kube-apiserver, the request goes through:

... -> Authentication -> Authorization -> Admission Controllers -> ...

Admission controllers help us to implement improved security mesarues, allowing us to apply access conditions which can't be checked with Authentication or Authorization:

- Permit only images from certain registry.
- Do not allow runAs root user.
- Permit certain capabilities.
- Enforce use of pod labels.

## Predefined Admission Controllers

Kubernetes ships a set of predefined Admission controllers:

- AlwaysPullImages: ensures that images are always pulled on pod creation.
- DefaultStoraClass:
- EventRateLimit: preventFlooding.
- NamespaceExists: rejects requests to unexistent namespaces.
- NamespaceAutoProvision: disabled by default.
- ...

### NamespaceExists

NamespaceExists is an Admission Controller enabled by default. When a request specifies a non existant namespace NamespaceExists Admission Controller rejects the request:

```shell
$ kubectl run nginx --image nginx --namespace blue
Error from server (NotFound): namespaces "blue" not found
```

### NamespaceAutoProvision

Autocreates a namespace if it does not exists. It is disabled byu default.

### NamespaceLifecycle

NamespaceExits and NamespaceAutoProvision admission controllers are deprecated and replaced by NamespaceLifeCycle admission controller.

The NamespaceLifecycle admission controller ensures that any requests made to a non-existent namespace are rejected, and it safeguards the default namespaces, including default, kube-system, and kube-public, from being deleted.

## View Enabled Admission Controllers

```shell
$ kube-apiserver -h | grep enable-admission-plugins
--enable-admission-plugins strings ...
```
or
```shell
$ kubectl exec kube-apiserver-controlplane -n kube-system -- kube-apiserver -h | grep enable-admission-plugins
--enable-admission-plugins strings ...
```
or
```shell
$ ps -ef | grep kube-apiserver | grep admission-plugins
```
### Enable Admission Controllers

Add the Admission Controller in the `--enable-admission-plugins` flag of the `kube-apiserver`:

```kube-apiserver.service
$ ExecStart=/usr/local/bin/kube-apiserver \\
...
--enable-admission-plugins=NodeRestriction,NamespaceAutoProvision
```

or

```/etc/kubernetes/manifests/kube-apiserver.yaml
apiVersion: v1
kind: Pod
medatada:
    creationTimestamp: null
    name: kube-apiserver
    namespace: kube-system
spec:
    containers:
    - command:
        - kube-apiserver
        - --authorization-modes=Node.RBAC
        ...
        - --enable-admission-plugins=NodeRestriction,
        image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3
        name: kube-apiserver
```

### Disable default plugins

Use the `--disable-admission-plugins` instead of `--enable-admission-plugins`.

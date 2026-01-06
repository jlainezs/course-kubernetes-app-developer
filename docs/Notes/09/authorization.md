# Authorization

Defines what a user (human or machine) can do after accessing the cluster. Ideally, any user should be granted with the minium permissions it needs to execute its tasks flawlessly.

Kubernetes offers several authorization methods:

- Node
- ABAC (attribute based access control)
- RBAC (role based access control)
- Webhook

## Authorization modes

### Node
The explanation given in the video is very poor.

Used for node internal tasks.

### Abac

Associate a user or group of user with a set or permissions. We manage Abac authorization using policy files.

A permission change implies to modify the policy definition file and restart the kube API server.

### Rbac

This authorization schema is based on roles. We define permission on roles and we assign users to those roles.

A change in the roles affects all role assigned users immediately.

### Webhook

Outsources authorization mechanisms.

### Some additional mechanisms

#### AlwaysAllow

Allows all requests without performing any authorization checks.

#### AlwaysDeny

Denies all requests.

## Configure authorization mode

We need to tell the kube-apiserver to use an authorization method using the `authotization-mode` flag when executing the kube-apiserver:

```
kube-apiserver --authorization-mode=AlwaysAllow
```

By default, it uses `AlwaysAllow`.

### Several modes

It is possible to use several authorization modes:

```
kube-apiserver --authorization-mode=Node,RBAC,Webhook
```
On that case, a request accepted if any of the modes accepts the request.
# Service account
Access the cluster for automated tasks.

```
$ kubectl create serviceaccount my-service-account
```

On service account creation, Kubernetes creates a token for the service account and stores it as a [secret](./secrets.md). This token can be used as a Bearer token for authentication on calls against the Kubernetes API.

For applications inside the cluster, service account tokens can be retrieved by mounting volumes on the pod and then read by the application.

Each namespace has a default token, which is mounted on a volume on each created pod. We can check it describing the pod.

The default token only allows basic queries to the Kubernetes API.

We can avoid the creation of a default token by noting it in the spec section of the pod definition file.

```yaml
spec:
    ..
    automountServiceAccountToken: false
    ..
```

## Tokens on v.1.24

ServiceAccount tokens must be created explicitly with

```shell
$ kubectl create token my-service-account
```


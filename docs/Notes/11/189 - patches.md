# Patches

# Patches

Kustomize patches are a way to modify existing Kubernetes resource configurations by making targeted changes. They allow users to customize YAML manifests without directly altering the base files, ensuring reusability and maintainability. Patches are provided as overlays and can be specified in various formats, including JSON and strategic merge patches.

A patch needs three parameters:

1. Operation type: add/remove/replace.
2. Target: what resource should this patch be applied on.
    - Kind
    - Version/Group
    - Name
    - Namespace
    - labelSelector
    - AnnotationSelector
3. Value: for add/replace operations, what is the value to be replaced or added.

## How patch are defined

Given this initial deployment configuration
```
apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    replicas: 1
    selector:
        matchLabels:
            component: api
    template:
        metadata:
            labels:
                components: api
        spec:
            containers:
                - name: nginx
                  image: nginx
```
This patch will change the name of the previous deployment
```
patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: replace
          path: /metadata/name
          value: web-deployment
```
And this patch will change the replicas of the initial sample
```
patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: replace
          path: /spec/replicas
          value: 5
```
### Patches syntax

There are two ways to define a patch:

Json 6902 patch

```
patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: replace
          path: /spec/replicas
          value: 5
```

Strategic Merge Patch
```
patches:
      patch: |-
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: api-deployment
        spec:
            replicas: 5
```

### Where to define patches

1. Inline patch definition: we add the patches in the `kustomization.yaml` file.
2. In a separate file referencing it in the kustomization.yaml file.
We can use Json 6902 notation
```kustomization.yaml
patches:
    - path: replicas-patch.yaml
      target:
        kind: Deployment
        name: nginx-deployment
```

```replicas-patch.yaml
- op: replace
  path: /spec/replicas
  value: 5
```
or strategic merge patch notation

```kustomization.yaml
patches:
    - path: replicas-patch.yaml
```

```replicas-patch.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    replicas: 5
```

## References

- [Json 6902 patch RFC](https://datatracker.ietf.org/doc/html/rfc6902)



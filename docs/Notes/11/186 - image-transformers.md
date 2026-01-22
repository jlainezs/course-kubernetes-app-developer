# Image transformers

Image transformers allow us to modify an image that a specific deployment or container is going to use through Kustomize.

This kustomization file
```kustomization.yaml
...
images:
  - name: nginx
    newName: haproxy
...
```
applied to the object definition
```
...
kind: Deployment
...
spec:
    spec:
        containers:
            - name: web
              image: nginx
```
will produce `spec.spec.containers[0].image: haproxy`

We can also change the image tag:
```kustomization.yaml
...
images:
  - name: nginx
    newTag: "2.4"
...
```
will produce `spec.spec.containers[0].image: nginx:2.4`

It is possible to change both:
```kustomization.yaml
...
images:
  - name: nginx
    newName: haproxy
    newTag: 2.4
...
```
will produce `spec.spec.containers[0].image: haproxy:2.4`

## Transformers applied to nested kustomization files

If we have a transformers defined on a root `kustomization.yaml` file and in a different kustomization.yaml in a subdirectory, all transofrmers defined on both files will applied to the resources imported.

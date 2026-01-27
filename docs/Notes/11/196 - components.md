# Compponents

Components provide the ability to define reusable pieces of confioguration logic that can be included in multiple overlays.

Components are useful in situations where applications support multiple optional features that need to be enabled only in a subset of overlays.

## Project structure

```
k8s/
    base/
        api-depl.yaml
        kustomization.yaml
    components/
        caching
            redis-depl.yaml
            deployment-patch.yaml
            kustomization.yaml
        db/
            postgres-depl.yaml
            deployment-patch.yaml
            kustomization.yaml
    overlays/
        dev/
            kustomization.yaml
        premium/
            kustomization.yaml
        standalons/
            kustomization.yaml
```

```k8s/components/db/kustomization.yaml
apiVersion: kustomize.k8s.io/v1alpha1
kind: Component

resources:
    - postgres-depl.yaml

secretGenerator:
    - name: postgres-cred
      literals:
        - password=postgres123

patches:
    - deployment-patch.yaml
```
## Component usage

```k8s/overlays/premium/kustomization.yaml
bases:
    - ../../base

components:
    - ../../components/db
```

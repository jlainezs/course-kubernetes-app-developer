# Overlays

Overlays help us to define different environemnts. Sarting from a base configuration, we'll apply some customizations to prepare different environments (dev, stg and prod).

P.e. we can organize our configurations like:

```
k8s/
    base/
        kustomization.yaml
        nginx-depl.yaml
        service.yaml
        redis-depl.yaml
    overlays/
        dev/
            kustomization.yaml
            config-map-yaml
        stg/
            kustomization.yaml
            config-map-yaml
        prod
            kustomization.yaml
            config-map-yaml
```

NOTE: we are not forced to any directory structure!

- **base**: share or default configurations across all environments.
- **overlays**: each environment customization (one folder per environment). Each env folder contains configuration customizations and patches of the base settings as well as specific configuration files.

```base/kustomization.yaml
resources:
    - nginx-depl.yaml
    - service.yaml
    - redis-depl.yaml
```
```dev/kustomization.yaml
bases:
    - ../../base

patch: |-
    - op: replace
      path: /spec/replicas
      value: 2
```


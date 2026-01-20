# Kustomize problem statement & idealogy

The problem to address is to customise different environments without replicating all configuration files on each environment.

## Overlays
Kustomize uses a **Base** file and an **overlay** for each environment. The overlay defines the properties we want to change on any of the environemnts.

## Folder structure

```
base/
    kustomization.yaml
    nginx-depl.yaml
    service.yaml
    redis-depl-yaml
overlays/
    dev
        kustomization.yaml
        config-map.yaml
    stg
        kustomization.yaml
        config-map.yaml
    prod
        kustomization.yaml
        config-map.yaml
```

Base contains the shared or default configurations for all environments.

Overlays contains each environment specific confituration  that add or modifies the base configuration.

**Kustomize** is shipped with **kubectl**, but it is not always the latest version.

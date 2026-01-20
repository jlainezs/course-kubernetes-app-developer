# kustomization.yaml file

Kustomize needs the `kustomization.yaml` file. We need to create that file manually.

In the `resoources` section we add the resources to be managed by kubernetes, that is, a list of yaml files

```
resources:
    - nginx-deployment.yaml
    - nginx-service.yaml
# Customizations we want to apply
commonLabels:
    company: KodeKloud
```
We assume we have created the referenced yaml files too.

## Build

Run the command

```
$ kustomize build k8s/
```

and it will show the resources we defined with the applied transformations we defined on each resource file.

## Apply

Kustomize does not applies the produced resources to the cluster. For that, we need to redirect the oiutput to the `kubectl apply` command.
 

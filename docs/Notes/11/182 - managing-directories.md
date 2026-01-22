# Managing directories

kubectl allows us to apply object definitions stored in the working directory, but it skips subdirectories. If we want to keep different parts of the application on its own directories, we need to execute kubectl on each directory and subdirectory on every modification (or delete).

## Centralize resources list

Kustomize may help on that point. We define a `kustomization.yaml` file with all the resource we need (note the use of relative paths)

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
    - api/api-depl.yaml
    - api/api-service.yaml
    - db/db-depl.yaml
    - db/db-service.yaml
```
Then apply changes is one command task:
```shell
$ kustomize build k8s/ | kubectl apply -f -
```

## Improve

What if the number of directories grows? We'll end with a hughe list of resources, making it difficult to manage.

A simplification we can reach with Kustomize is to define a  `kustomization.yaml` file on each subdirectory and note only the directory in the root's  `kustomization.yaml`

```kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
    - api/
    - db/
```

and

```api/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
    - api-depl.yaml
    - api-service.yaml
```
We also need an similar  `kustomization.yaml` file in the db directory.


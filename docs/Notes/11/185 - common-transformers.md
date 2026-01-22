# Common transformations

Kustomize common transformations are built-in features that allow users to efficiently modify Kubernetes manifests by applying consistent changes across multiple YAML files. These transformations include adding common labels, setting namespaces, and modifying resource names with prefixes or suffixes, all without manually editing each file.

- **customLabel**: adds a label to all Kubernetes resources.
- **namedPrefix/Suffix**: adds a common prefix/suffix to all resource names.
- **Namespace**: adds a common namespace to all resources.
- **commonAnnotations**: adds an annotation to all resources.

## Example

```kustomization.yaml
apiVersion: kustomize.config.k8s/v1beta1
kind: Kustomization

commonLabels:
    org: MyCompanyName
```

It will add `org:MyCompanyName` to all labels (in anywhere where labels are accepted) to the processed resources.

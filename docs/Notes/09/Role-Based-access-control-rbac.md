# Rol Based Access Control (RBAC)

## Create a Role

The following file definition will create the role "developer" with permissions for:

- View pods
- Create pods
- Delete pods
- Create ConfigMaps

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

For core groups, apiGroups can be left in blank.

Adding the `resourceNames` property we can also name resources to restrict the Role to some specific resources:

```
apiVersion: rbac.authorization.k8s.io/v1
...
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
  resourceNames: ["blue", "orange"]
...
```

## Bind user to role

Create a RoleBinding definition file

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
    kind: Role
    name: developer
    apiGroup: rbac.authorization.k8s.io

```

## Namespaces

Roles and RoleBindings are affected for namespaces. Previous examples, define objects to be created in the default namespace, as no namespace is given.

To specify a namespace, add the namespace in the metada section of the definition file:

```
apiVersion:...
metadata:
    namespace: namespace_name
```

## Viewing RBAC

Use the habitual `kubectl get` and `kubectl describe` command with `roles` and `rolebindings` subcommands.

## Check access
### Check my user
To check if my user can take some action:

```shell
$ kubectl auth cant-i create deployments
yes

$ kubectl auth cant-i delete nodes
no
```

### Impersonation
Administrators can impersonate users adding the `--as` flag followed by an user name on any (?) command.

To check permissions:

```shell
$ kubectl auth cant-i create deployments --as dev-user
no
```

or:

```shell
$ kubectl get deployments --as dev-user
Error from server (Forbidden): deployments.apps is forbidden: User "dev-user" cannot list resource "deployments" in API group "apps" in the namespace "default"
```

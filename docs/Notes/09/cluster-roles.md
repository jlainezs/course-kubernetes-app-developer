# Cluster roles

Cluster roles follow the same principles than roles, but they provide permissions for cluster wide operations.

The objectes managed with cluster roles are not namespaced objects, like nodes, persistent volumes or claims.

Per example, a cluster admin role could have permissions to view, create and delete nodes.

## Creation
### Cluster role
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "delete"]
```

```
$ kubectl create -f cluster-admin-role.yaml
```
### Bind user to role

Create a ClusterRoleBinding definition file

```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
    name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
    kind: ClusterRole
    name: cluster-administrator
    apiGroup: rbac.authorization.k8s.io

```
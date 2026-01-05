# KubeConfig

Access Kubernetes API using certificates:
Using curl:
```shell
curl https://my-kube:6443/api/v1/pods \
    --key admin.key
    --cert admin.crt
    --cacert ca.crt
```
or using kubectl
```shell
kucectl get pods
    --server my-kube:6443
    --client-key admin.key
    --client-certificate admin.crt
    --certificate-authority ca.crt
```
To avoid type those flags on next kubectl executions, we can specify a config file:
```
kubectl get pods
    --kubeconfig config
```
By default, kubectl looks for a configuration file in `$HOME/.kube/config`

## KubeConfig file
The config file has three sections:

- clusters: Define the different Kubernetes clusters you need to access to: dev, prod, Google, ...
- users: users define the user accounts used to access those clusters. Note that those users may have different privileges on differnt clusters: admin, developer, ...
- contexts: contexts express the relation between clusters and users: admin@prod, admin@google, developer@dev, ...

We are not creating users in the clusters, but using existing users already existing in the clusters.

The previous kubectl command fits in the config file:
- The  `server` and  `cacert` flag are placed under the clusters section.
- The  `client-key`, `client-certificate` flags are defined in the users section.
- Contexts allows to define wich user has access to which server.

### config example file

```
apiVersion: v1
kind Config

clusters:
- name: my-kube
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube:6443
- name: other
  ...
contexts:
- name: admin@my-kube
  context:
    cluster: my-kube
    user: admin
- name: admin@other
  ...
users:
- name: admin
  user:
    client-certificate: admin.crt
    client-key: admin.key
```
### Use a context
`config` has a specific field to define the active context:
```
apiVersion: v1
kind Config
current-context: admin@my-kube
clusters:
...
```
All commands which don't specify a context will use the `admin@my-kube` context.

### View kubeconfig files

`kubectl` allows us to view and modify config files:

```shell
$ kubectl config view

apiVersion: v1
kind: Config
current-context: admin@my-kube
...
```
will show the default config file contents. To view a specific kubeconfig file, use the `--kubeconfigfile`
```shell
$ kubectl config view --kubeconfig=my-custom-config

apiVersion: v1
kind: Config
current-context: admin@other
...
```

### Change default context
```shell
$ kubectl config use-context prod@my-kube

apiVersion: v1
kind: Config
current-context: prod@my-kube
...
```

Note the change in the `current-config` field.

### Other actions

From the  `kubectl config -h` command:

```
Available Commands:
  current-context   Display the current-context
  delete-cluster    Delete the specified cluster from the kubeconfig
  delete-context    Delete the specified context from the kubeconfig
  delete-user       Delete the specified user from the kubeconfig
  get-clusters      Display clusters defined in the kubeconfig
  get-contexts      Describe one or many contexts
  get-users         Display users defined in the kubeconfig
  rename-context    Rename a context from the kubeconfig file
  set               Set an individual value in a kubeconfig file
  set-cluster       Set a cluster entry in kubeconfig
  set-context       Set a context entry in kubeconfig
  set-credentials   Set a user entry in kubeconfig
  unset             Unset an individual value in a kubeconfig file
  use-context       Set the current-context in a kubeconfig file
  view              Display merged kubeconfig settings or a specified kubeconfig file

Usage:
  kubectl config SUBCOMMAND [options]
```

### Namespaces
When defining contexts, we can set a default namespace:
```
apiVersion: v1
kind Config

clusters:
...
contexts:
- name: admin@my-kube
  context:
    cluster: my-kube
    user: admin
    namespace: some-namespace-here
  ...
users:
...
```
### Certificates
It is recommended to use full paths to certificates.

Certificates can be specified using base64 encoded content instead of the file path.

```
apiVersion: v1
kind: Config

clusters:
- name: production
  certificate-authority-data: <base64_encoded_certificate_file_content>
...
```

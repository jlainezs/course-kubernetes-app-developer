# Helm

Bottom line: Helm is a "package manager"-like for Kubernetes. It manages what's called charts, which are pre-configured Kubernetes resources.

It is not part of Kubernetes and [it must be installed](https://helm.sh/docs/intro/install/#from-apt-debianubuntu) separatedly.

## Helm concepts

Example: Wordpress setup

There are several components involved: Service, Deployment, Secret, PV, PVC, Pods,. ..

With Helm we can parametrize those components.

```
apiVersion: v1
kind: Secret
metadata:
    name: wordpress-admin-password
data:
    username: XXXXX
    password: {{ .Values.passwordEnvoded }}
```
and define its values in a separete file
```values.yaml
passwordEnvoded: YYYYYYYYY
```

### Chart

A chart is a set of templates plus a values file. There's also a `chart.yaml` file which contains information about the chart itself.

### Repositories
In addition to create our own charts, charts can be found online at any chart repository like [ArtifactHub](https://artifacthub.io). We can also use CLI to find charts:

```shell
$ helm search hub wordpress
```

By default, this command will search in Artifact Hub repository. We can add additional repositories:

```shell
$ helm repo add bitnami https://charts.bitnami.com/bitnami
```
and search on it
```shell
$ helm search repo wordpress
```
Note the chage: repo instead of hub.

### Download a chart

To download a chart we run the pull command

```shell
$ helm pull --untar bitname/wordpress
```

This command will produce a folder, with the chart name, containing all chart files.

### Installation

To download and install a chart

```shell
$ helm install [release-name] [chart-name]
```
Each installation of a chart is called a release.
We can install the same chart on the same cluster using different release names.

We can install a chart from a local directory:

```shell
$ helm install [release-name] ./[directory-name]
```


We uninstall a release with the uninstall command:

```shell
$ helm uninstall [release-name]
```

# API Deprecations

Kubernetes API deprecation refers to the process of marking certain API versions as outdated and planning their removal in future releases. This is done to encourage users to transition to newer, more stable API versions, ensuring better functionality and security within the Kubernetes ecosystem.

## What is API Deprecation?

- Deprecation indicates that an API version is no longer recommended for use and will eventually be removed.
- Deprecated APIs still function for a period, allowing users time to transition to newer versions.
- The deprecation policy ensures users are informed about which APIs will be removed and when.

## Lifecycle of Kubernetes APIs

Kubernetes APIs follow a structured lifecycle:

| API Status	 | Description                                                                 |
|-------------|-----------------------------------------------------------------------------|
| Alpha       | Early testing phase; not stable for production.                             |
| Beta        | More stable than Alpha; still subject to changes.                           |
| Stable      | Fully tested and safe for production use; unlikely to change significantly. |

## API deprecation policy rules
### Rule nr.1

API elements may only be removed by incrementing the version of the API group.

To remove an API element, version group must be increased and then the API element can be removed from that new version:

```
/v1alpha1
     /op1
     /op2
```
to remove op2:
```
/v1alpha2
     /op1
```
but op2 is not removed from /v1alpha1

At the same time, the storage version is moved into v1alpha2

### Rule nr.2

API objects must be able to round-trip between API versions in a given release without information loss, with the exception of whole REST resources that do not exist in some versions.

### Rule nr. 3
An API version in a given track may not be deprecated until a new API version at least as stable release.

### Rule 4a

When an API is deprecated, users must migrate to a newer version, as stated in the Kubernetes deprecation policy

Other than most recent API versions in each track, older API versions must be supported after their announced deprecation for a duration of no less than:

- GA: 12 months or 3 releases (whichever is longer)
- Beta: 9 months or 3 releases (whichever is longer)
- Alpha: 0 releases

### Rule 4b

The preferred API and storage versions for a group can only be advanced once a release supporting both the new and previous versions is available.

## Kubectl Convert

Command line tool to convert API version object defition yaml files.

```
kubectl convert -f <old_file> --output-version <new-api>
```
It will take the `old_file` and dump it to console changing the API version.

``` shell
$ kubectl convert -f nginx-deploy.yaml --output-version apps/v1
```

### Convert command install

`convert` is a separate plugin, so it won't be available on your system and it need to be installed.

```
$ curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert
```
Note that a `kubectl-convert` file is created in the current directory. Set execution permission and move it:

```
root@controlplane:~# chmod +x kubectl-convert
root@controlplane:~#
root@controlplane:~# mv kubectl-convert /usr/local/bin/kubectl-convert
root@controlplane:~#
root@controlplane:~# pwd
```

## References

- [Deprecated API Migration Guide, Kubernetes documentation](https://kubernetes.io/docs/reference/using-api/deprecation-guide/)

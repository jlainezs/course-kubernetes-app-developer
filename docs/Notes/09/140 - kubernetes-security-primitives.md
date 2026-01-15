# Kubernetes security primitives

## Secure hosts

Access to hosts must be secured. Basic security principles recommend:

- Root access disabled.
- Password based authentication disabled.
- SSH key based authentication enabled.
- Additional security mechanisms:
    - Limited direct connectivity to shell.
    - Firewall protection.
    - Intrusion detection monitorization.
    - ...

## Secure Kubernetes

Risks and measures to take to secure a cluster. First step is to control access to the kube-apiserver by defining who can access and what can a user do.

### Authentication

Authentication checks who can access. There are several methods:

- Static token files.
- Certificates.
- External authentication providers (pe., LDAP).
- Service accounts (per machine).

### Authorization

With this point we define what the user can do:

- RBAC authorization.
- ABAC authorization.
- Node authorization.
- Webhook mode.

## TLS certificates

All cluster components and worker nodes components communication is secured using [TLS encryption](https://en.wikipedia.org/wiki/Transport_Layer_Security).

## Network policies

By default, all pods in a cluster can communicate with all pods. We can restrict that behaviour by defining network policies.

## References

- [Security, oficial Kubernetes documentation](https://kubernetes.io/docs/concepts/security/)

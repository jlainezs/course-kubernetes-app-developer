# Headless service

Consider the scenario of a MySQL cluster with master-multislave topology, like in [Stateful sets notes](./stateful-sets.md).

Services in pods are published with Kubernetes services, but in a deployment, the service acts like a load balancer accesssible using the DNS name of the service, and will allow to access to any of the pods, even slaves, which is not what we want to.

A hedless service does not balance requests but gives us DNS entries to reach each pod with the format pod_name.headless_service_name.namespace.domain. P.e., `mysql-0.mysql-h.default.svc.cluster.local`

## Creation

To create a headless service, we create a regular service specifying `spec.clusterIP` as `None`.

```
apiVersion: v1
kind: Service
metadata:
    name: mysql-h
spec:
    ports:
        - port: 3306
    selector:
        app: mysql
    clusterIP: None
```

In addition, pods should include some additional fields in the  `spec` section:

```
apiVersion: v1
kind: Pod
...
spec:
    subdomain: mysql-h
    hostname: mysql-pod
```
In the same way, Statefulsets should define the `spec.serviceName` value and the templates does not need to define  `template.spec.subdomain` and `template.spec.hostname` attributes, as they are automatically filled on pod creation.

## References

[Headless service, Kubernetes official documentation](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services)

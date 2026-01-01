# Stateful sets

## Justification

Consider the scenario of a SQL Server where an application is writting any data and we need to set up an HA scenario. Without digging deeper on MySQL HA cluster configuration, consider the single master-multislave topology (write on master, read on any). Usually this setup needs the following steps:

1. Setup master and slaves.
2. Clone data from mastere to slave-1.
3. Enable continuous replication from master to slave-1.
4. Wait for slave-1 to be ready.
5. Clone data from slave-1 to slave-2.
6. Enable continuous replication from master to slave-2.

### Deployment is not enough

Deployment can't warrant the order of the steps enumerated before, because on a deployment all needed pods are created at once.

Data cloning relies on having pefectly identified the master server, but using a deployment, every time a node is created, it gets a new IP and it may get a new name, while we are going to need a static name to refer to it from the slaves. Additionally, if the master pod crashes, slave pods will point to a wrong pod name when the master is recreated by Kubernetes.

That's why stateful sets are need and why they complement deployments.

### Characteristics

Stateful sets, like deployments, allow us to:

- Create pods using templates.
- Scale up and down.
- Perform rolling updates and rollbacks.

but there are some differences:

- Pods are created in a sequential order. When a pod gets into ready state, the stateful set deploys the following pod.
- Stateful set uses an index (starting at 0 and increments by one) to designate pods. Each pod gets a unique name combining this index and the stateful set name. On pop recreation the new pod will get the same name of the pod it replaces. On our case will end with mysql-0, mysql-1 and mysql-2.
- Pods are destroyed in creation reverse order when it scales down or it is being terminated.

## Usage

A stateful set is created like a deployment.

```stateful-set.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
    name: mysql
    labels:
        app: mysql
spec:
    template:
        metadata:
            labels:
                app: mysql
        spec:
            container:
            - name: mysql
              image: mysql
    replicas: 3
    selector:
        matchLabels:
            app: mysql
    serviceName: mysql-h
    podManagementPolicy:
```
In addition we add the `spec.serviceName` attribute. StatefulSet allows you to relax its ordering guarantees while preserving its uniqueness and identity guarantees via its  `.spec.podManagementPolicy` field.

- OrderedReady Pod Management: OrderedReady pod management is the default for StatefulSets. It implements the behavior described in Deployment and Scaling Guarantees.
- Parallel Pod Management: Parallel pod management tells the StatefulSet controller to launch or terminate all Pods in parallel, and to not wait for Pods to become Running and Ready or completely terminated prior to launching or terminating another Pod. For scaling operations, this means all Pods are created or terminated simultaneously.

## References
- [StatefulSets, on official documentation](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

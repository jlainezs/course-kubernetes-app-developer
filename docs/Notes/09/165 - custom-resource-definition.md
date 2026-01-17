# Custom resource definition

A Kubernetes Custom Resource Definition (CRD) allows users to extend the Kubernetes API by defining their own resource types, which behave like built-in Kubernetes objects. This enables better management of specific application needs and is essential for implementing Kubernetes Operators that automate application lifecycle management.

## Controllers
Built in resources (depployments, services, pods, ...) are managed by controllers, which monitors the etcd db for changes and keep the system on an expected state.

A custom resource can be, literally, anything. It requires a custom controller which implements the different actions.

```go
package flightticket

var controllerKind = apps.SchemeGroupVersion.WithKind("FlightTickiet")

// ... more code here

// Run begins watching and syncing
func (dc *FlightTicketController) Run(workers int, stopCh <-chan struct{})

// ... more code here

// Call BookFlightAPIReplicaSet
func (dc *FlightTicketController) callBookFlightApi(obj interface{})

// ... more code here
```

## Define resources with CRD (Custom Resource Definition)

Before creating custom resources we need to define them with a CRD definition.

```yaml
apiVersion: apiExtensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
    name: flighttickets.flights.com
spec:
    scope: Namespaced
    group: flights.com
    names:
        kind: FlightTicket
        singular: flightticket
        plural: flighttickets
        shortNames:
            - ft
    versions:
        - name: v1
          server: true
          storage: true
          schema:
            openAPIV3Schema:
                type: object
                properties:
                    from:
                        type: string
                    to:
                        type: string
                    number:
                        type: integer
                        minimum: 1
                        maximum: 10
```
With this CRD we can now create a FlightTicket resource:
```
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
    name: my-flight-ticket
spec:
    from: Mumbai
    to: London
    number: 2
```
Now the resource can be stored in the etc database and managed using kubectl

```shell
$ kubectl create -f ./flight.yaml
$ kubectl get flightticket
$ kubectl delete flightticket
```
We created some object, but to do something, we need to define a custom controller.

## Reference

- [Kubernetes Custom Resources, official documentation](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
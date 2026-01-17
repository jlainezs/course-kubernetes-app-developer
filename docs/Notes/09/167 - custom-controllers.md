# Custom controllers

We'll use the flightticket CRD defined in [custom resource definition](./165%20-%20custom-resource-definition.md)

We need to monitor of created objects and do some actions to manage real flight bookings.

## Introducing custom controller
A controller is any process or code that runs in a loop and is continuously monitoring the Kubernetes cluster and listening to events of specific objects being changed, in this case, the flight ticket object.

## Build a custom controller
As Kubernetes is implemented in Golang, it is better to stick to it and reuse existing resources in the Kubernetes Go Client, like caching, queues, shared informers.

Start with a sample controller [^sample-controller] by cloning the repository and modify the file `controller.go` file with our needs.

##  Run it

Run the custom controller on its own process:

```shell
$ ./sample-controller -kubeconfig=$HOME/.kube/config
```

## References

[^sample-controller]: [Kubernetes sample controller](https://github.com/kubernetes/sample-controller)
[^build-custom-controllers]: [Building Custom Controllers in Kubernetes: A Comprehensive Guide](https://wafatech.sa/blog/devops/kubernetes/building-custom-controllers-in-kubernetes-a-comprehensive-guide/)

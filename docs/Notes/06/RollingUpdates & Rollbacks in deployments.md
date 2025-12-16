# Rolling Updates & Rollbacks in Deployments

## Versioning

When a deployment is created it triggers a rollout. A new rollout creates a new replicaset, which is recorded as a new deployment revision.

Those revisions help us to keep track of changes and allows us to rollback revisions.

We can see the status of the rollout with the command

```shell
$ kubectl rollout status deployment/myapp-deployment
```

To see revisions and history of the deployment:

```shell
$ kubectl rollout history deployment/myapp-deployment
```

## Deployment strategy

### Recreate strategy
First approach is to destroy all running application instances and create new instances using the new deployment.

There's an "application down" period when old instances are destroyed.

This is not the default deployment strategy.

On deployment, the describe replicaset command shows in the events that the pod instances are all scaled up or down at the same time.

### Rolling update strategy
Instances are progressively replaced by new versions:
- Take one instance and destroy it
- Create a new version instance to replace the destroyed version

This is the default deployment strategy.

On deployment, the describe replicaset command shows in the events that the pod instances are scaled down and up one by one.

## Upgrades

A new deployment creates a new replicaset. Pods on new version are created according the deploymnet strategy. The new replicaset is visible after the deployment. 

## Rollback

```shell
$ kubectl rollout undo deployment/myapp-deployment
```

## Additional "strategies"
### Blue/Green
We have two versions up, with current version accepting all application traffic. In a certain point in time (pe, when all tests are passed over new version), traffic is switched to new version all at once.

With native kubernetes mechanisms this kind of deployment is implemented using services.

```service.yaml
apiVersion: v1
kind: Service
metadata:
    name: my-service
spec:
    selector:
        version: v1
```

With the deployment of v2 ready, we simply change the  `version` tag in the service definition to route all traffic to the new service.

```service.yaml
apiVersion: v1
kind: Service
metadata:
    name: my-service
spec:
    selector:
        version: v2
```

### Canary

We have to versions up serving different traffic weight. Old version serves most traffic, new version serves few traffic. When we decide that new version is OK, we simple apply a rolling update on old version and the new version serving few traffic is destroyed.

This is the deployment for v1 (note the sepc.template labels)

```v1.yaml
apiVersion: apps/v1
kind: Deployment
...
spec:
    template:
        metadata:
            labels:
                app: front-end
                version: v1
replicas: 5
selector:
    matchLabels:
        app: front-end
```
This is the deployment for v2 (note the sepc.template labels)

```v2.yaml
apiVersion: apps/v1
kind: Deployment
...
spec:
    template:
        metadata:
            labels:
                app: front-end
                version: v2
replicas: 1
selector:
    matchLabels:
        app: front-end
```
And this is the canary service. We are using the  `app` label to select nodes from v1 and v2

```canary.yaml
apiVersion: v1
kind: Service
metadata:
    name: my-service
spec:
    selector:
        app: front-end
```

The deployment definition, tells the number of running pods for each version (`replicas` parameter of the deployment definition file).

When v2 is ready, we route all traffic from v1 to v2 and remove v1:

```shell
$ kubectl scale deployment --replicas=0 v1
$ kubectl scale deployment --replicas=5 v2
$ kubectl delete deployment v1
```

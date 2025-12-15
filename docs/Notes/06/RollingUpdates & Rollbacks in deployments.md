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
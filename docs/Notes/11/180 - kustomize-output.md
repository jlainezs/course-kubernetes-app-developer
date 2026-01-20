# Kustomize output

To apply the Kustomize output we execute the following command

```
$ kustomize build k8s/ | kubectl apply -f -
service/nginx-loadbalancer-service created
deployment.apps/nginx-deployment created
```
or using the shipped `kustomize` version
```
$ kubectl apply -k k8s/
service/nginx-loadbalancer-service created
deployment.apps/nginx-deployment created
```

We pipe the output of kustomize to kubectl.

To delete the resources we pipe the kustomize output to `kubectl delete`

```
$ kustomize build k8s/ | kubectl delete -f -
```
or
```
$ kubectl delete -k k8s/
```

# API Groups

API groups are used to "group" features to manage and run the Kubernetes cluster.

We can check API groups with

 `curl https://localhost:6443 -k`

## Core group
All API core groups follow the structure:

 `/api/v1/api_group`

 where:
 - api: fixed
 - v1: api version. Usually is v1.
 - api_group: any of
    - namespaces
    - events
    - bindings
    - configmaps
    - pods
    - endpoints
    - pv
    - secrets
    - rc
    - nodes
    - pvc
    - services

## Named group APIs

New features are going to be made available through this groups. The URL structure of this APIs is slightly different from the core API:

 `/apis/api_group/version`

 An example:

`/apis/apps/v1/deployments`

## Direct access

Accessing the API directly may return unauthorized access errors. To avoid them,

1. we need to send credentials with the request (using certificates)
2. or launch a proxy  `kubectl proxy` to use the credentials on the config file. Then use curl to connect to the API using the proxy `curl http://localhost:8001`. The example show how to use the default proxy port.   

## Reference

- [API Referece, Kubernetes official documentation](https://kubernetes.io/docs/reference/)

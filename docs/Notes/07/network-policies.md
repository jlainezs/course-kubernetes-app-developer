# Network policies

## Basics
By default Kubernetes cluster allows a all-to-all communication schema, so any pod can reach any other pod in the cluster.

Network polcies are used to define rules to alter the default networking behaviour on a cluster. A network policy can be used to define which pods are allowed to communicate with a certain pod.

Network policies are enforced by the network solution implemented on Kubernetes cluster. Not all implementations support policies.

### Ingress and egress

This concepts are defined from the pod pov.

- Ingress: requests.
- Egress: responses

### Rules
Labels and selectors are the basic piece used to define policy rules.

The following ingress rule will allow traffic from pods labeled with api-pod to port 3306. This policy will be applied to pods labeled `role: db`

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: db-policy
spec:
    podSelector:
        matchLabels:
            role: db
    policyTypes:
    - Ingress
    ingress:
    - from:
        - podSelector:
            matchLabels:
                name: api-pod
        ports:
        - protocol: TCP
          port: 3306
```

## Network policies

### Namespace selector
To allow pods from certain namespace we'll use the namespaceSelector. The following policy will allow connections from `api-pod` in the `prod` namespace.

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
    name: db-policy
spec:
    podSelector:
        matchLabels:
            role: db
    policyTypes:
    - Ingress
    ingress:
    - from:
        - podSelector:
            matchLabels:
                name: api-pod
          namespaceSelector:
            matchLabels:
                kubernetes.io/metadata.name: prod
        ports:
        - protocol: TCP
          port: 3306
```

We can allow ingress traffic from services outside of the kluster by defining `ipBlock` rules in the `from` section of the policy:

```
    - from:
        - ipBlock:
            cidr: 192.168.5.10/32
```
This rule will allow ingress traffic from the ip `192.168.5.10/32`.

### OR ingress traffic

Note the dash on the `namespaceSelector`:

```
    - from:
        - podSelector:
            matchLabels:
                name: api-pod
        - namespaceSelector:
            matchLabels:
                kubernetes.io/metadata.name: prod
        - ipBlock:
            cidr: 192.168.5.10/32
```

This way, the expression to allow traffic is podSelector OR nameSpaceSelector OR ipBlock

### AND ingress traffic

Note the missing dash on the `namespaceSelector` and  `ipBlock`:

```
    - from:
        - podSelector:
            matchLabels:
                name: api-pod
          namespaceSelector:
            matchLabels:
                kubernetes.io/metadata.name: prod
          ipBlock:
            cidr: 192.168.5.10/32
```

This way, the expression to allow traffic is podSelector AND nameSpaceSelector AND ipBlock

## Egress policy

```
...
policyTypes:
- Ingress
- Egress
ingress:
    ...
egress:
- to:
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 80
```


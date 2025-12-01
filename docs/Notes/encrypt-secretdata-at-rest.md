# Encrypt secret data
Secrets are created and its value is encoded in base64, which is easyly decoded.


```$ kubectl create secret generic my-secret \
        --from-literal=key1=supersecret
$ get secret my-secret -o yaml
...
data:
  key1: c3VwZXJ...
...

$ echo "c3VwXXJ..." | base64 --decode
supersecret
```
Data is stored in plain text in the etcd server. To check it:

1. Install ```etcdctl``` if not available.
2. Run
    ```$ ETCDCTL_API=3 etcdctl \
            --cacert=/etc/kubernetes/pki/etcd/ca.crt \
            --cert=/etc/kubernetes/pki/etcd/server.crt \
            --key=/etc/kubernetes/pki/etcd/server.key \
            get /registry/secrets/default/my-secret | hexdump -C
       ...
       000000f0 ..... |key1..supersecre|
       00000100 ..... |t..Opaque.."..|
       0000010e .....
       ...
    ```
## Check if the encryption provider is running

### Check if the kube api is running with encryption provider
```
$ ps -aux | grep kube-api | grep "encryption-provider-config
```

### Check the kubernetes setup manifest
Additionally, ,we can check the kubernetes setup manifest at ```/etc/kubernetes/manifests/kube-apiserver.yaml``` Look for encryption-provider option in the command section of the file.

## An encryption configuration file sample

```
---
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - identity: {} # plain text, in other words NO encryption
      - aesgcm:
          keys:
            - name: key1
              secret: c2VjcmV0IGlzIHNlY3VyZQ==
            - name: key2
              secret: dGhpcyBpcyBwYXNzd29yZA==
      - aescbc:
          keys:
            - name: key1
              secret: c2VjcmV0IGlzIHNlY3VyZQ==
            - name: key2
              secret: dGhpcyBpcyBwYXNzd29yZA==
      - secretbox:
          keys:
            - name: key1
              secret: YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXoxMjM0NTY=
```
Order in providers matters. As identity is the first method, it is the one that will be used on ecryption. For decription, if one method fails, the system tries the next one.

## References
- [Kubernetes documnentation, Encrypt Confidential Data at Rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)

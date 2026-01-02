# Authentication

Several users with different roles can access the cluster: admins, developers, application end users, bots, ... Application end users are not relevant for Kubernetes authentication, as they are internally managed by applications.

Kubernetes does not manage users accounts natively. It relies on external sources like certificates or identity providers.

Kubernetes allows to create service accounts.

## Auth mechanisms

To restrict `kube-apiserver`

- Static token file
- Certificates
- Identity services (ldap, kerberos, ...)

## Static token file

The simplest control is to define a token file with the format

token,user,uid,"group1,group2,group3"

``` csv
alkskjhfdku3jkfhm,user1,1123,test,dev
kjasdfgjkewyucxmj12,user2,566,dev
lskdfjldskfnkjhf,user3,7654
```

To use it, we need to pass it to the kube-apiserver with `--token-auth-file=static_tocken_file.csv` and the requests must include a bearer token

```shell
curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer alkskjhfdku3jkfhm"
```

**WARNING**: Static token file authentication mechanism is not recommended and it is deprecated on Kubernetes v1.19


## References

- [Authentication, Kubernetes official documentation](https://pwittrock.github.io/docs/admin/authentication)

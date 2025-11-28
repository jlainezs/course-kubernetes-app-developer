# Secrets

Application sensitive data, like passwords.

## Creation

### From command line

```shell
kubectl create secret generic app-secret \
    --from-literal=DB_HOST=mysql \
    --from-literal=DB_USER=root \
    --from-literal=DB_PWD=password
```
Secrets can be defined in a file

```shell
kubectl create secret generic app-secret \
    --from-file=app.secret_properties
```
```app.secret_properties
DB_HOST: mysql
DB_USER: root
DB_PWD: password
```
### From declarative file

```shell
kubectl create -f ./secret-data.yaml
```
```secret-data.yaml
apiVersion: v1
kind: Secret
metadata:
    name: app-secret
data:
    DB_HOST: mysql
    DB_USER: root
    DB_PWD: password
```

It is convenient to 'hide' the secret values. One way to do it is obfuscate them.  P.e., we can use the value expressed in base64 (not very safe tbh):

```bash
echo -n 'the text to encode' | base64
```

## View secrets

```shell
kubectl get secrets
```

```shell
kubectl describe secrets
```
Describe shows the keys but hides the values. To view the secrets value require the secret and formats the output to yaml.

```shell
kubectl get secret app-secret -o yaml
```

## Configure the pod

### Inject secret as environment variable
One way to use secrets in the pod is to inject them as environment variables.

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      envFrom:
        - secretRef:
            name: app-secret
```

### Inject secrets using a volume

```
valumes:
- name: app-secret-valume
  secret:
    secretName: app-secret
```

Then we need one file for each secret. Each file contains the secret value.

```shell
$ ls /opt/app-secret-volume
DB_HOST DB_PASSWORD DB_USER

$ cat  /opt/app-secret-volume/DB_HOST
mysql
```


# Validating and mutating admission controllers

- Admission Controllers: accept or reject a request (pe., NamespaceExists).
- Mutating Admission Controllers: introduce some changes in the request (pe., DefaultStorage class).
- Mixed: Mutate and Validate requests.

## Admission controllers execution order

1. Mutating Admission Controllers
2. Admission Controllers

This way allows to introduce changes and validate them lately.

## Customized Admission Controllers

Kubernetes offers the hooks

- MutatingAdmissionWebhook
- ValidatingAdmissionWebhook

We can configure those webhooks to point to a server with the admission controller running on it.

Webhooks are executed after all built-in admission controllers are executed. Kubernetes passes to the Webhook an Admission Review Object, which contains all request details.

The webhook responds with an Admission Review Result Object with a result, telling if the request is allowed (`response.allowed`).

## Webhook Configuration

1. Deploy the amission webhook server
2. Create a webhook configuration object

### Deploy webhook server

It is an API server built on any stack. It should process POST requests to /validate and /mutate endpoints, accepting Admission Review Object and returning an Admission Review Result Object.

### Webhook configuration object

Note that communication is done using TLS, so a pair of certificates are required.

```
apiVersion: admissionregistration.k8s.io/v1
kind: validatingWebhookConfiguration
metadata:
    name: "pod-policy.example.com"
webhooks:
- name: "pod-policy.example.com"
  clientConfig:
    # if deployed externally to the cluster
    # url: "https://external-server.example.com"
    # otherwise
    service:
        namespace: "webhook-namespace"
        name: "webhook-service"
    caBundle: "Ci0tLS0tQk...tLS0K"
  rules:
  - apiGroups: [""]
    apiVersions: ["v1"]
    operations: ["CREATE]
    resources: ["pods"]
    scope: "Namespaced"
```

## References

- [Admission Control in Kubernetes, official](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)

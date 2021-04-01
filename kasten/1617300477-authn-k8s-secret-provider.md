# 1617300477 authn-k8s-secret-provider

```yaml
- !host
  id: conjur-secret-provider
  annotations:
    authn-k8s/namespace: conjur
    authn-k8s/authentication-container-name: cyberark-secrets-provider-for-k8s

- !grant
  role: !group conjur/authn-k8s/<authn-id>/apps
  members:
  - !host conjur-secret-provider
```


## Links

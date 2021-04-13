# 1618333199 conjur-authn-oidc-simple-policy

```yaml
# root.yml

# authenticators
- !policy
  id: conjur/authn-oidc/cyberark
  body:
  - !webservice
  - !variable provider-uri
  - !variable id-token-user-property
  - !group users
 
  - !permit
    role: !group users
    privilege: [ read, authenticate ]
    resource: !webservice

# oidc users
- &oidc-users
  - !user andrew
  - !user foo
  - !user bar

- !grant
  role: !group conjur/authn-oidc/cyberark/users
  members: *oidc-users
```


## Links

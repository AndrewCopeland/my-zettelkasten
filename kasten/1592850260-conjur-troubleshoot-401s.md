# 1592850260 conjur-troubleshoot-401s
#conjur #audit #troubleshoot #401

Perform the following command:
```bash
curl -H "$header" -s -k "${CONJUR_APPLIANCE_URL}/audit" | jq | grep "failed to authenticate"
```

The output should look like:
```
"message": "conjur:host:something failed to authenticate with authenticator authn-iam service conjur:webservice:conjur/authn-iam/global: CONJ00006E 'host/something' does not have 'authenticate' privilege on conjur:webservice:conjur/authn-iam/global"
    "message": "conjur:host:something failed to authenticate with authenticator authn-iam service conjur:webservice:conjur/authn-iam/global: CONJ00004E 'authn-iam/global' is not enabled"
    "message": "conjur:host:something failed to authenticate with authenticator authn-iam service conjur:webservice:conjur/authn-iam/global: CONJ00004E 'authn-iam/global' is not enabled"
```


## Links

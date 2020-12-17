# 1608230175 ory-keto-testing

Check if service is alive:
```bash
curl localhost:4466/health/alive
```

List all roles inside of keto:
```bash
curl localhost:4466/engines/acp/ory/exact/roles
```


Create a role:
```bash
data='{
  "id": "get-all-secrets",
  "description": "role for getting all secrets",
  "members": [
  ]
}'

curl -X PUT localhost:4466/engines/acp/ory/exact/roles -H 'Accept: application/json' -H 'Content-Type: application/json' --data "$data"
```

Give a role permissions to a resource:
```bash
data='{
  "id": "get-all-secrets",
  "actions": [
    "read",
    "read-secret",
    "list"
  ],
  "description": "Read and list all secrets",
  "effect": "allow",
  "resources": [
    "secrets:*"
  ],
  "subjects": [
    "get-all-secrets"
  ]
}'

curl -X PUT localhost:4466/engines/acp/ory/exact/policies -H 'Accept: application/json' -H 'Content-Type: application/json' --data "$data"
```


## Links

# 1608230175 ory-keto-testing

Check if service is alive:
```bash
curl localhost:4466/health/alive
```

List all roles inside of keto:
```bash
curl localhost:4466/engines/acp/ory/glob/roles
```


Create a role:
```bash
data='{
  "id": "get-all-secrets",
  "description": "role for getting all secrets",
  "members": [
  ]
}'

curl -X PUT localhost:4466/engines/acp/ory/glob/roles -H 'Accept: application/json' -H 'Content-Type: application/json' --data "$data"
```

Create a policy attached to a role:
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

curl -X PUT localhost:4466/engines/acp/ory/glob/policies -H 'Accept: application/json' -H 'Content-Type: application/json' --data "$data"
```


List all policies:
```bash
curl localhost:4466/engines/acp/ory/glob/policies
```

Assinging an entity as a member of a role:
```bash
data='{
  "members": [
    "entity:andrew"
  ]
}'

curl -X PUT localhost:4466/engines/acp/ory/glob/roles/get-all-secrets/members  --data "$data"
```

At this point subject `entity:andrew` should have the actions `read, read-secret and list` on resource `secrets:*`.


To validate this lets give it a shot:
```bash
data='{
  "action": "read",
  "resource": "secrets:some-secret",
  "subject": "entity:andrew"
}'
curl -X POST localhost:4466/engines/acp/ory/glob/allowed --data "$data"
```


```
// Entity commands
Entity().HasAccess(action string, resource string, entity string) bool

// Role Commands
// When a role is created so is its corresponding policy. 
// Each role will have a 1-to-1 mapping between it and a policy
Role().Create(roleID string, desc string, effect string, resources []string) error
Role().Delete(roleID string) error
Role().AddMember(roleID string, entity string) error
Role().RemoveMember(roleID string, entity string) error

// Secret commands
Secret().Create(data Secret) (Secret, error)
Secret().Read(secretID string) (Secret, error)
Secret().ReadSecret(secretID string) (Secret, error)
Secret().ReadAll(secretID string) (Secret, error)
Secret().List(secretFolderID string) ([]string, error)
Secret().Delete(secretID string) error

// Authenticator commands
Authenticator().Create(data Authenticator)
Authenticator().Delete(authenticateID string)

// Group commands
Group().Create(groupID string, desc string)
Group().Delete(groupID string)
Group().AddRole(groupID string, roleID string)
Group().RemoveRole(groupID string, roleID string)
Group().AddEntity(groupID string, entityID string)
Group().RemoveEntity(groupID string, entityID string)
```

## Links

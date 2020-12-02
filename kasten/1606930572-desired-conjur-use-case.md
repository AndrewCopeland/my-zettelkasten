# 1606930572 desired-conjur-use-case
A resource is any object than can have permissions associated with them. In this case we will have the following resources:
- secret: A complex data structure containing data and sensative data and will also include information like expirtation and creation date
- role: A given role maps a resource to a permission. An entity that is a member of a role will inherit its permissions 
- entity: Is an identity or specific application. This could be a user or a application or an automation server.
- authenticator: Is mapped to a entity, an entity can authenticate with an authenticator.


## Secret
A secret object will looks like:
```json
{
	"id": "appteam1:databases:postgres:on-prem-1",
	"data": {
		"username": "app12345",
		"address": "10.2.1.5",
		"port": "22"
	},
	"secret_data": {
		"api_key": "hjsdjsjsjs89383hd828h2d8hd",
		"cert": "ContentOfAMultilinedCertificate\nGoes\nHere"
	},
	"creation_time": 16000871,
	"last_modified_time":  16000971,
	"version": 2,
	"expiration_time": 170000896
}
```

- `id` is the ID of the secret, it is delimited by `:`. `role` section will have more information on why delimiter is important
- `data` is a flat dictionary containing non-sensative keys and values.
- `secret_data` is a flat dictionary containing sensative keys and values. This data will be encrypted in the backend.
- `creation_time`: EPOCH of creation of secret
- `last_modified_time`: EPOCH of last time secret was modified
- `version`: Secrets can only be overridden meaning all secrets are versioned
- `expiration_time`: The time in which this secret will not longer be retrievable for any entity.


## Role
A role object will look like:
```json
{
	"id": "appteam1:databases:postgres:on-prem-1:read",
	"resource": "secret:appteam1:databases:postgres:on-prem-1",
	"permissions": [
		"read",
		"read-secret",
		"list"
	]
}
```
- `id` is the ID of the role. 
- `resource` is the target resource this role will have permissions on.
- `permissions` is the permission this role has on the specified resource


In this case, any entity with this role will have the ability to `read, read-secret & list` secret `appteam1:databases:postgres:on-prem-1`

To give less granular control, I could create a role that has access to list and read all secrets in `appteam1:databases` by creating the following role:
```json
{
	"id": "appteam1:read-all-databases",
	"resource": "secret:appteam1:databases:*:*",
	"permissions": [
		"read",
		"read-secret",
		"list"
	]
}
```

For an entity to create the role above it will need the following roles associated with them:
```json
{
	"id": "entity:andrew:roles",
	"resource": "role:appteam1:*",
	"permissions": [
		"create",
	]
}
```

To apply a role to an entity the following API call must be made:
```
POST /entity/andrew/apply-role
{
  "roles": [
	  "entity:andrew:roles"
  ]
}
```

As you can see we have the ability to apply many roles to one entity using one API call. Of course the entity executing this API call must have the following roles:

Permission to `apply-role` to the `andrew` entity.
```json
{
	"id": "apply-role-to-andrew",
	"resource": "entity:andrew",
	"permissions": [
		"apply-role"
	]
}
```

Permission to `role` the `entity:andrew:roles` role:
```json
{
	"id": "role-to-andrew-roles",
	"resource": "role:entity:andrew:roles",
	"permissions": [
		"role"
	]
}
```

Same permissions to the resource the role contains:
```json
{
	"id": "create-appteam1-roles",
	"resource": "role:appteam1:*",
	"permissions": [
		"create",
	]
}
```


For `entity:admin` to apply `role:entity:andrew:roles` to `entity:andrew` a 3 way validation must occur:
1. `entity:admin` must have `role` permission on `role:entity:andrew:roles`
2. `entity:admin` must have `apply-role` permission on `entity:andrew`
3. `entity:admin` must have `create` permission on `role:appteam1:*`

## Links

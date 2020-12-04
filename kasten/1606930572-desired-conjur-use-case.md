# 1606930572 desired-conjur-use-case
A resource is any object than can have permissions associated with them. In this case we will have the following resources:
- secret: A complex data structure containing data and sensative data and will also include information like expirtation and creation date
- role: A given role maps a resource to a permission. An entity that is a member of a role will inherit its permissions 
- entity: Is an identity or specific application. This could be a user or a application or an automation server.
- authenticator: Is mapped to a entity, an entity can authenticate with an authenticator.
- group: A grouping of roles in which an entity can be a member of.


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

Permission to `apply` the `entity:andrew:roles` role:
```json
{
	"id": "role-to-andrew-roles",
	"resource": "role:entity:andrew:roles",
	"permissions": [
		"apply"
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
		"role"
	]
}
```


For `entity:admin` to apply `role:entity:andrew:roles` to `entity:andrew` a 3 way validation must occur:
1. `entity:admin` must have `apply` permission on `role:entity:andrew:roles`
2. `entity:admin` must have `apply-role` permission on `entity:andrew`
3. `entity:admin` must have `role` permission on `role:appteam1:*`
4. `entity:admin` must have `create` permission on `role:appteam1:*`

## Entity
An entity object will look like:
```json
{
  "id": "andrew",
  "created": 1606938260,
  "expires": -1,
  "authenticators": [
	  {
		  "id": "api-key",
		  "config": {
			  "max_ttl": 300
		  }
	  }
  ],
  "roles": [
	  "entity:andrew",
	  "app-team1:read-all-secrets"
  ]
}
```

In this case there is an entity `andrew` which will never expire and was created at `1606938260`. Entity `andrew` has the ability to authenticate via `api-key` and the access token provided has a maximum time to live of 300 seconds (5 mins). Entity `andrew` is also a member of roles: `entity:andrew` and `app-team1:read-all-secrets`.  The `authenticators` section will describe authenticators more indepth. I will display the roles above to describe what exactly `andrew` can do.

The following are the expanded roles applied to `andrew`:
```json
[
	{
		"id": "entity:andrew",
		"resources": [
			"role:andrew:*",
			"secret:andrew:*",
			"entity:andrew:*"
		],
		"permissions": [
			"*"
		]
	},
	{
		"id": "app-team1:read-all-secrets",
		"resource": "secret:app-team1:*:*:*",
		"permissions": [
			"read",
			"list",
			"read-secret"
		]
	}
]

```


- role `entity:andrew` represents any permissions on folders `role:andrew:`, `secret:andrew:` and `entity:andrew:`. Any entity that has this role applied to them will have the ability to create secrets, entities and roles. However this user will only be able to do these actions within the `andrew` namespace.

- role `app-team1:read-all-secrets` represents the ability to read all secrets inside of the `app-team1` folder and 3 sub folders below. This role does not give the ability to `create` or `delete`.


NOTE: Should an entity have the ability to have more than one authenticator? I think that this is probably not neccessary however I might have 1 application that is deployed in many different authenticators........


## Authenticator
An authenticator object is defined below:
```json
{
	"id": "api-key-default",
	"type": "api-key",
	"config": {
		"mac_ttl": 1440
	}
}
```


Authenticators objects are more dynamic than other objects because each authenticator `type` will define a different `config` section. This validation will occur on the backend.


An example of a k8s authenticator is below:
```json
{
	"id": "k8s-cluster-1",
	"type": "k8s",
	"config": {
		"url": "https://k8s-cluster-1.on-prem.company.com",
		"cert": "secret:authenticator:k8s-cluster-1:cert",
		"api_key": "secret:authenticator:k8s-cluster-1:api_key"
	}
}
```

When `k8s-cluster-1` authenticator is used the authenticator will use the `url`, `cert` and `api_key` to establish a connection to the k8s cluster and validate the entitiy attempting to authenticate.


Authenticator can be applied to specific `entities` by assigning `roles` to the entities.


NOTE: I envision defining a `authenticator` to an `entity` when the `entity` is created or applying the `authenticator` to the `entity` after creation. However this should be done via roles, which means how does one define the `config` to the authenticator. Should a `role` object contain a `config` section? This is slightly confusing however it would allow the ability to define authentication methods in a granular or generic way.



## Group

TBD: However it should just be a collection of roles. When an `entity` is a member of a `group` it will inherit all of the `roles` that make up the `group`.









## Links

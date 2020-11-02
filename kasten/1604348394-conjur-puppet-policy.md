# 1604348394 conjur-puppet-policy

```yaml
- &puppet-masters
  - !host puppet-master-1

- &puppet-agents
  - !host epic-prod-app1

- &agent-secrets
  - !variable db/username
  - !variable db/password

- &master-secrets
  - !variable agent/epic-prod-app1

# granting puppet master ability to
# read puppet agent api key secrets
- !permit
  roles: *puppet-masters
  resources: *master-secrets
  privileges: [ read, execute ]

# granting agents ability to read
# the secrets they require
- !permit
  roles: *puppet-agents
  resources: *agent-secrets
  privileges: [ read, execute ]
```



## Links
- [1603725828-puppet-dual-api-key-implementation.md](1603725828-puppet-dual-api-key-implementation.md)

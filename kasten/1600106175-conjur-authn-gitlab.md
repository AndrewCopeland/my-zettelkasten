# 1600106175 conjur-authn-gitlab

The following policy should be loaded to configure the gitlab authenticator:
```yaml
- !policy
  id: conjur/authn-gitlab/global
  body:
  - !webservice
  - !group apps
  - !variable tenant-id
  - !permit
    role: !group apps
    resource: !webservice
    privileges:
    - authenticate
```

Depending on if you are using gitlab in the cloud or on-prem the `tenant-id` would be different.
By default the cloud `tenant-id` is `gitlab.com`.

To define a specific gitlab job within conjur load the following policy:
```yaml
- !host
  id: some-gitlab-job
  annotations:
    authn-gitlab/namespace: andcope1995
    authn-gitlab/repo: test-runner
    # Branch could be used to give different levels of permissions depending on the branch
    # authn-gitlab/branch: master, dev, etc....
```


Now within the actual pipeline convert your gitlab JWT to a conjur access token. Then use this conjur access token to retrieve your needed secrets.
```bash
conjur_token=$(curl --data "$CI_JOB_JWT" https://conjur.company.local/authn-gitlab/global/account/host%2Fsome-gitlab-job/authenticate)
header="Authorization: Token token=\"$(echo $conjur_token | base64)\""

secret1=$(curl -H "$header" https://conjur.company.local/secrets/account/variable/path/to/secret1)
secret2=$(curl -H "$header" https://conjur.company.local/secrets/account/variable/path/to/secret2)
```

For more information of what is actually in the [`CI_JOB_JWT` environment variable see here.](1600105851-gitlab-runner-jwt.md)

## Links
- [1600105851-gitlab-runner-jwt.md](1600105851-gitlab-runner-jwt.md)
- [1600103673-gitlab-setup-docker-compose.md](1600103673-gitlab-setup-docker-compose.md)

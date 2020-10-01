# 1601577457 conjur-concourse-integration-bosh-ops-file

Below is an example of a OPs file for deploying Conjur /w bosh.
```yaml
# ops file
- path: /instance_groups/name=web/jobs/name=web/properties/conjur?
  type: replace
  value:
    appliance_url: https://conjur-master
    account: companyName
    tls:
      ca_cert: |
        <certificate contents>
    auth:
      login: host/concourse-instance
      api_key: 283y749823bf9uh23n97rh23f792h379fh2347942f
    secret_template: vaultName/lobUser/CONCOURSE_SAFE
```


## Links

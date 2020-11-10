# 1605030782 conjur-seed-fetcher-policy
```yaml
- !policy
  id: conjur/seed-generation
  body:
  # This webservice represents the Seed service API
  - !webservice
  - !group consumers
  # Authorize `consumers` to request seeds
  - !permit
    role: !group consumers
    privilege: [ "execute" ]
    resource: !webservice
```



## Links

# 1591301273 conjur-authn-k8s-external-authenticator-follower-in-k8s

We have to disable a couple of things within our manifest.

Add the following in your deployments pod spec:
```yaml
automountServiceAccountToken: false
enableServiceLinks: false
```

On the Conjur container you must set 2 environment variables:
```yaml
- name: KUBERNETES_SERVICE_HOST
  value: ""
- name: KUBERNETES_SERVICE_PORT
  value: ""
```

When the follower starts up, it should authenticate to the master as it normally would with seed fetcher. But now applications from other k8s clusters will be able to authenticate against this follower.


## Links
- [1586875794-conjur-authn-k8s-external-authenticator.md](1586875794-conjur-authn-k8s-external-authenticator.md)

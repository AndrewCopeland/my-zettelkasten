# 1592504888 conjur-config-map-k8s
#conjur #k8s #configmap

A config map when intergrating k8s clustet with conjur:
```yaml
# Holds DAP config info for apps in all namespaces
# Access is gained via rolebinding to a clusterrole
apiVersion: v1
kind: ConfigMap
metadata:
  name: dap-config
data:
  CONJUR_ACCOUNT: dev
  CONJUR_MASTER_HOST_NAME: ocp-lab-master.northcentralus.cloudapp.azure.com
  CONJUR_MASTER_URL: https://ocp-lab-master.northcentralus.cloudapp.azure.com
  CLUSTER_AUTHN_ID: ocp4lab
  CONJUR_VERSION: "5"
  CONJUR_APPLIANCE_URL: https://conjur-follower.cybrlab.svc.cluster.local
  CONJUR_AUTHN_URL: https://conjur-follower.cybrlab.svc.cluster.local/api/authn-k8s/ocp4lab
  CONJUR_AUTHN_TOKEN_FILE: /run/conjur/access-token
  CONJUR_MASTER_CERTIFICATE: |
    -----BEGIN CERTIFICATE-----
    MIID/TCCAuWgAwIBAgIUAkgdczWRMjEM6JAmdvCMqHZTiw0wDQYJKoZIhvcNAQEL
    ...
    -----END CERTIFICATE-----
  CONJUR_FOLLOWER_CERTIFICATE: |
    -----BEGIN CERTIFICATE-----
    MIIDujCCAqKgAwIBAgIUS8M98fvfZIYyLae33qNwJB/HsqkwDQYJKoZIhvcNAQEL
    ...
    -----END CERTIFICATE-----
```



## Links
